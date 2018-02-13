# Android Style Guide

## Dependencies
- [Java Style Guide](https://github.com/whitecloakph/java-style-guide)

## Inspiration

This style-guide is somewhat of a mash-up between the existing Java language style guides. The language guidance is drawn from the following:

+ [Android Buffer App Style Guide](https://github.com/bufferapp/android-guidelines)
+ [Ribot Android Style Guide](https://github.com/ribot/android-guidelines)

## Table of Contents

- [File Naming](#file-naming)
  - [Class Files](#class-files)
  - [Resource Files](#resource-files)
    - [Drawable Files](#drawable-files)
    - [Layout Files](#layout-files)
    - [Menu Files](#menu-files)
- [XML Style Rules](#xml-style-rules)
  - [Use self closing tags](#use-self-closing-tags)
  - [Resources Naming](#resource-naming)
    - [IDs](#id)
    - [Strings](#strings)
    - [Styles and Themes](#styles-and-themes)
  - [Attributes ordering](#attributes-ordering)
- [Gradle Style](#gradle-style)
  - [Dependencies](#dependencies)
    - [Versioning](#versioning)
    - [Grouping](#grouping)
    - [Independent Dependencies](#independent-dependencies)

## Project Guidelines

### File Naming

### Class Files

Class names are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

For classes that extend an Android component, the name of the class should end with the name of the component; for example: `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.

### Resource Files

Resources file names are written in __lowercase_underscore__.

#### Drawable Files

Naming conventions for drawables:

| Asset Type   | Prefix            |		Example                  |
|--------------| ------------------|-----------------------------|
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`	           | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          |
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`	           | `ic_star.png`               |
| Menu         | `menu_	`          | `menu_submenu_bg.9.png`     |
| Notification | `notification_`	 | `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

Naming conventions for icons (taken from [Android iconography guidelines](http://developer.android.com/design/style/iconography.html)):

| Asset Type                      | Prefix             | Example                      |
| --------------------------------| ----------------   | ---------------------------- |
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

Naming conventions for selector states:

| State	       | Suffix          | Example                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |

#### Layout Files

Layout files should match the name of the Android components that they are intended for but moving the top level component name to the beginning. For example, if we are creating a layout for the `SignInActivity`, the name of the layout file should be `activity_sign_in.xml`.

| Component        | Class Name             | Layout Name                   |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `activity_user_profile.xml`   |
| Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`        |
| Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml`  |
| AdapterView item | ---            	    | `item_person.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |

A slightly different case is when we are creating a layout that is going to be inflated by an `Adapter`, e.g to populate a `ListView`. In this case, the name of the layout should start with `item_`.

Note that there are cases where these rules will not be possible to apply. For example, when creating layout files that are intended to be part of other layouts. In this case you should use the prefix `partial_`.

#### Menu Files

Similar to layout files, menu files should match the name of the component. For example, if we are defining a menu file that is going to be used in the `UserActivity`, then the name of the file should be `activity_user.xml`

A good practice is to not include the word `menu` as part of the name because these files are already located in the `menu` directory.

#### Values Files

Resource files in the values folder should be __plural__, e.g. `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

## XML Style Rules

### Use self closing tags

When an XML element doesn't have any contents, you __must__ use self closing tags.

This is good:

```xml
<TextView
	android:id="@+id/text_view_profile"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content" />
```

This is __bad__ :

```xml
<!-- Don\'t do this! -->
<TextView
    android:id="@+id/text_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```

### Resources naming

Resource IDs and names are written in __lowercase_underscore__.

#### ID

IDs should be prefixed with the name of the element in lowercase underscore. For example:

| Element            | Prefix            |
| -------------------|-------------------|
| `TextView`         | `text_`           |
| `ImageView`        | `image_`          |
| `Button`           | `button_`         |
| `Menu`             | `menu_`           |

Image view example:

```xml
<ImageView
    android:id="@+id/image_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Menu example:

```xml
<menu>
	<item
        android:id="@+id/menu_done"
        android:title="Done" />
</menu>
```

#### Strings

String names start with a prefix that identifies the section they belong to. For example `registration_email_hint` or `registration_name_hint`. If a string __doesn't belong__ to any section, then you should follow the rules below:


| Prefix               | Description                           |
| ---------------------|---------------------------------------|
| `error_`             | An error message                      |
| `msg_`               | A regular information message         |
| `title_`             | A title, i.e. a dialog title          |
| `action_`            | An action such as "Save" or "Create"  |


#### Styles and Themes

Unlike the rest of resources, style names are written in __UpperCamelCase__.

### Attributes ordering

As a general rule you should try to group similar attributes together. A good way of ordering the most common attributes is:

1. View Id
2. Style
3. Layout width and layout height
4. Other layout attributes, sorted alphabetically
5. Remaining attributes, sorted alphabetically

## Gradle Style

### Dependencies

#### Versioning

Where applicable, versioning that is shared across multiple dependencies should be defined as a variable within the dependencies scope. For example:


    final SUPPORT_LIBRARY_VERSION = '23.4.0'

    compile "com.android.support:support-v4:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:recyclerview-v7:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:support-annotations:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:design:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:percent:$SUPPORT_LIBRARY_VERSION"
    compile "com.android.support:customtabs:$SUPPORT_LIBRARY_VERSION"

This makes it easy to update dependencies in the future as we only need to change the version number once for multiple dependencies.

#### Grouping

Where applicable, dependencies should be grouped by package name, with spaces in-between the groups. For example:


    implementation "com.android.support:percent:$SUPPORT_LIBRARY_VERSION"
    implementation "com.android.support:customtabs:$SUPPORT_LIBRARY_VERSION"

    implementation 'io.reactivex:rxandroid:1.2.0'
    implementation 'io.reactivex:rxjava:1.1.5'

    implementation 'com.jakewharton:butterknife:7.0.1'
    implementation 'com.jakewharton.timber:timber:4.1.2'

    implementation 'com.github.bumptech.glide:glide:3.7.0'


`implementation` , `testImplementation` and `androidTestImplementation`  dependencies should also be grouped into their corresponding section. For example:


    // App Dependencies
    implementation "com.android.support:support-v4:$SUPPORT_LIBRARY_VERSION"
    implementation "com.android.support:recyclerview-v7:$SUPPORT_LIBRARY_VERSION"

    // Instrumentation test dependencies
    androidTestImplementation "com.android.support:support-annotations:$SUPPORT_LIBRARY_VERSION"

    // Unit tests dependencies
    testImplementation 'org.robolectric:robolectric:3.0'

Both of these approaches makes it easy to locate specific dependencies when required as it keeps dependency declarations both clean and tidy 🙌


#### Independent Dependencies

Where dependencies are only used individually for application or test purposes, be sure to only compile them using `compile` , `testCompile` or `androidTestCompile` . For example, where the robolectric dependency is only required for unit tests, it should be added using:


    testImplementation 'org.robolectric:robolectric:3.0'
