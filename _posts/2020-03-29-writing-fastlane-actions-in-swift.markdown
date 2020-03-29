---
layout: post
title:  "Writing fastlane plugins in Swift"
date:   2020-03-29 17:26:05 +0100
categories: garbage
---

# Writing fastlane plugins in Swift

Let's say that you want to create a custom fastlane plugin but you don't want
to stop using Swift. Since fastlane currently do not support plugins for Swift
in any form we will have to do all the heavy lifting ourselves. But it's not
that difficult. As an example we are just going to make a simple swift library
that uses `PersonNameComponentsFormatter` to format names and expose that to
fastlane.

## 1. Creating the plugin

Start by creating and initializing the plugin by running

```bash
fastlane new_plugin person_name_formatter
fastlane add_plugin
```

This generates all the relevant files you will need for your plugin. Make sure
to also initialize a git repository for this folder.

## 2. Creating the Swift library

In the root folder of your new plugin. Create a new folder named `SwiftActions`.
Here run `swift package init --type library`. This will generate everything
that you need to compile your swift library.

Edit your `Package.swift` file and make sure that you compile it to a _dynamic_
library. Your file should look like this:

```swift
// swift-tools-version:5.1

import PackageDescription

let package = Package(
    name: "SwiftActions",
    products: [
        .library(
            name: "SwiftActions",
            type: .dynamic, // <- Important!
            targets: ["SwiftActions"]),
    ],
    dependencies: [],
    targets: [
        .target(
            name: "SwiftActions",
            dependencies: []),
        .testTarget(
            name: "SwiftActionsTests",
            dependencies: ["SwiftActions"]),
    ]
)
```

Now you can add the functionality you want in your plugin in `SwiftActions/Sources/SwiftActions/SwiftAction.swift`

Keep in mind that you can only interop with ruby via the C api. So you will
need to use C-compatible types.

```swift
import Foundation

public typealias CString = UnsafePointer<CChar>

// Declare what the function name will be in C
@_cdecl("format_name")
public func format(name sName: CString) -> CString {
    let name = String(cString: sName)
    if #available(macOS 10.12, *) {
        let formatter = PersonNameComponentsFormatter()
        guard
            let components = formatter.personNameComponents(from: name)
        else {
            return ("" as NSString).utf8String!
        }

        formatter.style = .abbreviated

        return (formatter.string(from: components) as NSString).utf8String!
    }
    return ("" as NSString).utf8String!
}

```

## 3. Update the Rakefile

Now we will have to add a build step in our ruby build system. Edit the Rakefile
and add the following build step.

```ruby
require 'bundler/gem_tasks'

require 'rspec/core/rake_task'
RSpec::Core::RakeTask.new

require 'rubocop/rake_task'
RuboCop::RakeTask.new(:rubocop)

task(:build_swift) do
  # Build SwiftActions
  Dir.chdir('SwiftActions') do
    `swift build --configuration=release`
  end
  # Move the dynamic library to the `bin` folder.
  FileUtils.mkdir_p('./bin')
  FileUtils.cp('./SwiftActions/.build/release/libSwiftActions.dylib', './bin/')
end

# Build the swift project before doing anything else.
task(default: [:build_swift, :spec, :rubocop])
```

If you run `bundle exec rake` it should now build your swift library and
move it to `bin/libSwiftActions.dylib`.

## 4. Update the action

First we will have to include the gem `fiddle`. In `fastlane-plugin-person_name_formatter.gemspec`
add the line `spec.add_dependency('fiddle')` and install by running `bundle install`.

The last piece of the puzzle is to update our actual fastlane action.
in the file `lib/fastlane/plugin/person_name_formatter/actions/person_name_formatter_action.rb`
add the following:

```ruby
require 'fastlane/action'
require_relative '../helper/person_name_formatter_helper'
require 'fiddle/import'

module Fastlane
  module SwiftActions
    extend Fiddle::Importer
    dlload './bin/libSwiftActions.dylib'
    extern "char* format_name(char *)"
  end
  module Actions
    class PersonNameFormatterAction < Action
      def self.run(params)
        formatted = SwiftActions.format_name(params[:name])
        UI.message("Hi #{formatted}")
        formatted
      end

      def self.description
        "Formats a persons name"
      end

      def self.authors
        ["Niil Öhlin"]
      end

      def self.return_value
        "Returns the name abbrivated"
      end

      def self.details
        ""
      end

      def self.available_options
        [
          FastlaneCore::ConfigItem.new(key: :name,
                               description: "Name to format",
                                  optional: false,
                                      type: String)
        ]
      end

      def self.is_supported?(platform)
        true
      end
    end
  end
end

```

You can now try to run your action by running

```bash
$ bundle exec fastlane run person_name_formatter name:'Niil Öhlin'
[✔] 🚀
+---------------------------------------+---------+-----------------------+
|                              Used plugins                               |
+---------------------------------------+---------+-----------------------+
| Plugin                                | Version | Action                |
+---------------------------------------+---------+-----------------------+
| fastlane-plugin-person_name_formatter | 0.1.0   | person_name_formatter |
+---------------------------------------+---------+-----------------------+

[18:39:18]: -----------------------------------
[18:39:18]: --- Step: person_name_formatter ---
[18:39:18]: -----------------------------------
[18:39:18]: Hi NÖ
[18:39:18]: Result: NÖ
```

Damn that's a lot of work. But it works!
