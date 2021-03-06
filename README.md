# Xmartlabs' Android Style Guide

This is the Xmartlabs' Android and Java Styles Guide. These are conventions in general followed by the team.

To set it up, download the [Android Studio style file](intellij-java-xl-style.xml) and copy it to `config/codestyles` in your Android Studio settings folder. 
For Mac OS you should copy and paste the same file to `Users/me/Library/Preferences/AndroidStudio1.5/codestyles`. Check that the folder is the correct Android Studio version, and if necessary, create the codestyles folder.

Then in Android Studio, in `Settings`/`Code` select it. Note that this configuration enforce most of the points found in this guide, but not all.



You can reformat the code automatically by pressing `Ctrl + Alt + L` (default keyboard shortcut for Reformat Code). Is also recommended to commit with git via Android Studio and that Reformat Code is enabled when doing this.

## Java

The idea is to follow [Google Java Style](http://google.github.io/styleguide/javaguide.html), but with some changes and adding ideas.

### Changes

#### Column limit

We use 120 as the column limit. Not 80, nor 100.

#### Vertical whitespace

There must be no vertical whitespace before the first member and after the last one of a class.

```java
public class ClassExample {
  private int firstMember; // no vertical whitespace before this line

  // ...

  public void lastMember() {
    // ...
  } // no vertical whitespace after this line
}
```

Multiple vertical whitespaces must never be used.

#### Empty blocks

Empty blocks are preferently closed immediately after it is opened, with no characters or line break in between, unless it is part of a multi-block statement (one that directly contains multiple blocks: if/else or try/catch/finally).

[Google Empty block preferences](http://google.github.io/styleguide/javaguide.html#s4.1.3-braces-empty-blocks)

#### Array initializers

The preferred array initializer "block-like construct" style when it doesn't fit in one line is one value per line:

```java
new int[] {
  0,
  1,
  2,
  3,
}
```

#### Numeric literals

`float`-valued literals use a lowercase `f` suffix: `100.23f`.

#### Class and interface names

If it's a list of an entity, use singular: `CustomerListFragment` for instance.

#### Field, method, variable and parameter names

If you need to name a `List` of activities for example, just name it `activities` and if it's `ListView`: `activitiesListView`. So we use plural here.

#### Local variable names

One-character names are only permitted if they are `i`, `j` or `k` indexes in loops. They must be used in this respective order when nesting them. Avoid using these loops indexes outside of them too. Example:

```java
for (int i = 0; i < n1; i++) {
  for (int j = 0; j < n2; j++) {
    for (int k = 0; k < n3; k++) {
      // ...
    }
  }
}
```

#### Type variable names

A single capital letter must be used, such as `T`, `S` or `D`.

#### Where Javadoc is used

Javadoc must be taken into account only in projects that are an API, open source or if it's explicitly requested. It can also be used in places that looks necessary in order to clarify certain behavior.

### Additions

#### Conditional operator

The conditional (ternary) operator may be used if the conditions and its corresponding values are considered to be simple enough. The readability of the code must not be disturbed. Also, it must not be used nested. If it doesn't fit in one line, it could be up to 3 lines, being one line for the condition, one for the first value and one for the second one. But if it doesn't fit in 3 lines in this format, it shouldn't be used. Example:

```java
// This is okay:
boolean minValue = value1 < value2 ? value1 : value2;

// This is okay:
int someValue = value1 < value2 && value1 < value3 && value1 < value4 && value1 < value5 && value1 < value6
    ? value1 + value2 + value3 + value4
    : value2 + value3 + value4 + value5;

// This isn't okay:
String userHasProperty = user != null && Objects.equals(user.getProperty(), "a sample value")
    ? String.valueOf("%s %s %s", "Some", "very very very very very very very very very complicated",
    "and very very very very long string")
    : (user != null && user.satisfiesCondition() ? "this string" : "this other string");
```

#### Ask for the positive

Following Martin Fowler's [Remove Double Negative](http://www.refactoring.com/catalog/removeDoubleNegative.html) refactoring, try to make the conditionals single positive.

#### Interfaces naming

Interfaces names must **not** have an `I` prefixed, like in `IComparable` (just `Comparable` is preferred). In the particular case of having an interface and an abstract class that implements it giving a default behavior, prefix the class with the word `Abstract`. Examples of this can be found in the Java API: `Queue` and `AbstractQueue`, `Set` and `AbstractSet`, among others.

#### Increment and decrement operators

Inline use of increment and decrement operators should be avoided. The outcome of the calculation is less easily predictable. Example:

```java
// This is fine:
int aValue = otherValue * someValue + 1;
someValue++

// This does not:
int aValue = otherValue * someValue++ + 1;
```

#### Inner classes and interfaces

Only use them when they refer to something private to the class (only the class will use it) and they are small in the number of lines. Also when there is no other choice than they are non-static and when it's an interface related to a callback (like `OnClickListener` inside of `View`).

### Nullable and NonNull

Try to use wherever you can the `@Nullable` and `@NonNull` annotations from the [Support Annotations](http://tools.android.com/tech-docs/support-annotations). Besides doing this messes up the code, it helps to avoid `NullPointerException` with the help of Android Studio's lint.

### Usage of TODO and FIXME

Where possible, use `TODO` and `FIXME` comments. Write a date when using them. When using IntelliJ IDEA and Android Studio, you just press "todo" or "fixme" and it lets you autocomplete with a message. Use `TODO` when you are leaving something to be done that you are not going to do now and `FIXME` for something similar but related to a bug.

## Android

### Alphabetic order where possible

Always you have something that order is the same from the machine perspective, prefer to sort it alphabetically. An example of this the methods order in a builder construction, or the enum values order, if the enum value ordinal doesn't care and if the order doesn't go along with some logical order.

### Fragments and Activities classes naming

These classes must be named after the use case they satisfy, suffixed by `Fragment` or `Activity`. Examples are: `CustomerListFragment` and `RepoDetailActivity`. Fragments that are used only as pages in `ViewPager`s, as if they were "weak entities" (they shouldn't exist outside it) must be called with the suffix `PageFragment`, such as in `TutorialStep1PageFragment`.

### Class fields order

Fields go first. They are logically grouped in this order: constants, Activities extras, Fragments arguments, views, dependency-injected members and then plain fields. Additional subgroups can be defined.

### Resources

We follow the [Resources section of the Futurice's Android best practices](https://github.com/futurice/android-best-practices#resources), with the following adjustments.

#### Layouts

Regarding the naming we use underscores instead of dots. Also, we add that list item views follow the convention of prefixing with `item`, as in `item_customer.xml`. If naming a list header (header item or header of the whole list), use the prefix `header`.

Attributes with a namespace come first. Between those, first `id` (if present), then `layout_width` and then `layout_height`. Then the rest alphabetically. After this, the ones without namespace, such as `style`, alphabetically.

Namespaces declarations shall be the first lines in the root element and must be ordered alphabetically. The name for `http://schemas.android.com/apk/res/android` must be `android`, `http://schemas.android.com/apk/res-auto` must be `app` and the name for `http://schemas.android.com/tools` must be `tools`.

A single vertical whitespace must be left before a closing tag.

The naming of the ids of the views is the following. First, if it applies, try to use Android provided ids. This applies mainly for `ListView`s, `RecyclerView`, lists items `TextView`s and so on. If it doesn't apply, the name must be the context associated, then the name and finally the view name. All snake cased, except for the view name. Example: `customer_list_description_textView`. If a layout needs identification, it's better to suffix it with just `layout`, as normally `relativeLayout` or similar won't contribute much. Also, if naming a place where a fragment will be added, name it or suffix it with `fragment_container`.

#### Styles

The style files should be split in different files if the size of the project is big and it deserves it. Also, use styles in views only if they are reused.

The naming should be prefixed with the view name followed by a dot. Logical organization of different names (as namespaces) should be separated with dots too.

#### Colors

The color palette shouldn't have many elements. So it's okay for color names to be `red`, `orange` and so on (intituively inferred). If having repeated colors, use versions like `redLight`, `redLighter`, `redDark` and `redDarker`. There shouldn't be much more (it might reflect a design problem). For alpha versions of other colors, prefer the convention `red20` (being 20 the percentage of opacity, in decimal). For transparent tinted versions, use something like `transparentBlackRed`.

#### Strings

Split the strings with dots based on words that give context. For example `user.company_description` is cool, but `user.company.description` doesn't (unless company is a sort of namespace). Give context based on the view and/or the use case.

Short strings, such as "Do", "Yes", "Do it", "Got it", "Settings", "Go back" and "Support and Feedback" must be named with their value: `do`, `yes`, `do_it` and so on.

If you had the bad luck and the name of the string is a Java reserved word, prefix it with an underscore.

#### Arrays

String arrays should always use string resources and not string literals in its items. This makes translations easy, as arrays are not translated, but the strings do. This avoids problems of having arrays with different size or order in different languages.

```xml
<!-- This is cool -->
<string-array name="time_granularity">
    <item>@string/day</item>
    <item>@string/week</item>
    <item>@string/month</item>
</string-array>

<!-- This is not -->
<string-array name="time_granularity">
    <item>day</item>
    <item>week</item>
    <item>month</item>
</string-array>
```

The name of the arrays should not contain the word `array`, as they are in their own array namespace already.

Also, all arrays should be in a separate file from strings.

## Misc

### Unix newline convention

As the [3.206 Line of the POSIX standard](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_206) establishes, all files must be ended with a newline.
