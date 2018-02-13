# Android Style Guide

## Dependencies
- [Java Style Guide](https://github.com/whitecloakph/android-style-guide)

## Inspiration

This style-guide is somewhat of a mash-up between the existing Java language style guides. The language guidance is drawn from the following:

+ [Android Buffer App Style Guide](https://github.com/bufferapp/android-guidelines)
+ [Ribot Android Style Guide](https://github.com/ribot/android-guidelines)

## File Naming

### Class

Class names are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

For classes that extend an Android component, the name of the class should end with the name of the component; for example: `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.

### Resource

Resources file names are written in __lowercase_underscore__.

### Drawable

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

### Layout

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

### Menu

Similar to layout files, menu files should match the name of the component. For example, if we are defining a menu file that is going to be used in the `UserActivity`, then the name of the file should be `activity_user.xml`

A good practice is to not include the word `menu` as part of the name because these files are already located in the `menu` directory.

### Values

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

### ID naming

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

### Strings

String names start with a prefix that identifies the section they belong to. For example `registration_email_hint` or `registration_name_hint`. If a string __doesn't belong__ to any section, then you should follow the rules below:


| Prefix               | Description                           |
| ---------------------|---------------------------------------|
| `error_`             | An error message                      |
| `msg_`               | A regular information message         |
| `title_`             | A title, i.e. a dialog title          |
| `action_`            | An action such as "Save" or "Create"  |


### Styles and Themes

Unlike the rest of resources, style names are written in __UpperCamelCase__.

### Attributes ordering

As a general rule you should try to group similar attributes together. A good way of ordering the most common attributes is:

1. View Id
2. Style
3. Layout width and layout height
4. Other layout attributes, sorted alphabetically
5. Remaining attributes, sorted alphabetically

## Java Language Rules for Android

### Never ignore exceptions

Avoid not handling exceptions in the correct manner. For example:

	public void setUserId(String id) {
    	try {
        	mUserId = Integer.parseInt(id);
    	} catch (NumberFormatException e) { }
	}

This gives no information to both the developer and the user, making it harder to debug and could also leave the user confused if something goes wrong. When catching an exception, we should also always log the error to the console for debugging purposes and if necessary alert the user of the issue. For example:


	public void setCount(String count) {
    	try {
        	count = Integer.parseInt(id);
    	} catch (NumberFormatException e) {
    		count = 0;
        	Log.e(TAG, "There was an error parsing the count " + e);
        	DialogFactory.showErrorMessage(R.string.error_message_parsing_count);
    	}
	}

Here we handle the error appropriately by:

- Showing a message to the user notifying them that there has been an error
- Setting a default value for the variable if possible
- Throw an appropriate exception

### Never catch generic exceptions

Catching exceptions generally should not be done:


	public void openCustomTab(Context context, Uri uri) {
    	Intent intent = buildIntent(context, uri);
    	try {
        	context.startActivity(intent);
    	} catch (Exception e) {
        	Log.e(TAG, "There was an error opening the custom tab " + e);
    	}
	}

Why?

*Do not do this. In almost all cases it is inappropriate to catch generic Exception or Throwable (preferably not Throwable because it includes Error exceptions). It is very dangerous because it means that Exceptions you never expected (including RuntimeExceptions like ClassCastException) get caught in application-level error handling. It obscures the failure handling properties of your code, meaning if someone adds a new type of Exception in the code you're calling, the compiler won't help you realize you need to handle the error differently. In most cases you shouldn't be handling different types of exception the same way.* - taken from the Android Code Style Guidelines

Instead, catch the expected exception and handle it accordingly:

	public void openCustomTab(Context context, Uri uri) {
    	Intent intent = buildIntent(context, uri);
    	try {
        	context.startActivity(intent);
    	} catch (ActivityNotFoundException e) {
        	Log.e(TAG, "There was an error opening the custom tab " + e);
    	}
	}


### Grouping exceptions

Where exceptions execute the same code, they should be grouped in-order to increase readability and avoid code duplication. For example, where you may do this:

	public void openCustomTab(Context context, @Nullable Uri uri) {
    	Intent intent = buildIntent(context, uri);
    	try {
        	context.startActivity(intent);
    	} catch (ActivityNotFoundException e) {
        	Log.e(TAG, "There was an error opening the custom tab " + e);
    	} catch (NullPointerException e) {
        	Log.e(TAG, "There was an error opening the custom tab " + e);
    	} catch (SomeOtherException e) {
    		// Show some dialog
        }
	}

You could do this:

	public void openCustomTab(Context context, @Nullable Uri uri) {
    	Intent intent = buildIntent(context, uri);
    	try {
        	context.startActivity(intent);
    	} catch (ActivityNotFoundException e | NullPointerException e) {
        	Log.e(TAG, "There was an error opening the custom tab " + e);
    	} catch (SomeOtherException e) {
    		// Show some dialog
        }
	}
	
### Using try-catch over throw exception

Using try-catch statements improves the readability of the code where the exception is taking place. This is because the error is handled where it occurs, making it easier to both debug or make a change to how the error is handled.

### Never use Finalizers

*There are no guarantees as to when a finalizer will be called, or even that it will be called at all. In most cases, you can do what you need from a finalizer with good exception handling. If you absolutely need it, define a close() method (or the like) and document exactly when that method needs to be called. See InputStreamfor an example. In this case it is appropriate but not required to print a short log message from the finalizer, as long as it is not expected to flood the logs.* - taken from the Android code style guidelines

### Fully qualify imports

When declaring imports, use the full package declaration. For example:

Don’t do this:


    import android.support.v7.widget.*;

Instead, do this 😃


    import android.support.v7.widget.RecyclerView;
    
### Don't keep unused imports

Sometimes removing code from a class can mean that some imports are no longer needed. If this is the case then the corresponding imports should be removed alongside the code

## Java Style Rules

### Field definition and naming

All fields should be declared at the top of the file, following these rules:


- Private, non-static field names should not start with m. This is right:

    userSignedIn, userNameText, acceptButton

Not this:

    mUserSignedIn, mUserNameText, mAcceptButton


- Private, static field names do not need to start with an s. This is right:

    someStaticField, userNameText

Not this:

	sSomeStaticField, sUserNameText


- All other fields also start with a lower case letter.


    int numOfChildren;
    String username;


- Static final fields (known as constants) are ALL_CAPS_WITH_UNDERSCORES.


    private static final int PAGE_COUNT = 0;

Field names that do not reveal intention should not be used. For example,

    int e; //number of elements in the list

why not just give the field a meaningful name in the first place, rather than leaving a comment!

    int numberOfElements;

That's much better!

### View Field Naming

When naming fields that reference views, the name of the view should be the last word in the name. For example:

| View           | Name              |
|----------------|-------------------|
| TextView       | usernameView      |
| Button         | acceptLoginView   |
| ImageView      | profileAvatarView |
| RelativeLayout | profileLayout     |

We name views in this way so that we can easily identify what the field corresponds to. For example, having a field named **user** is extremely ambiguous - giving it the name usernameView, userAvatarView or userProfieLayout helps to make it clear  exactly what view the field corresponds with.

Previously, the names for views often ended in the view type (e.g acceptLoginButton) but quite often views change and it's easy to forgot to go back to java classes and update variable names.

### Avoid naming with container types

Leading on from the above, we should also avoid the use of container type names when creating variables for collections. For example, say we have an arraylist containing a list of userIds:

Do:

    List<String> userIds = new ArrayList<>();

Don't:

    List<String> userIdList = new ArrayList<>();

If and when container names change in the future, the naming of these can often get forgotten about - and just like view naming, it's not entirely necessary. Correct naming of the container itself should provide enough information for what it is.

### Avoid similar naming

Naming variables, method and / or classes with similar names can make it confusing for other developers reading over your code. For example:

	hasUserSelectedSingleProfilePreviously

	hasUserSelectedSignedProfilePreviously

Distinguishing the difference between these at a first glance can be hard to understand what is what. Naming these in a clearer way can make it easier for developers to navigate the fields in your code.

### Number series naming

When Android Studio auto-generates code for us, it's easy to leave things as they are - even when it generate horribly named parameters! For example, this isn't very nice:

	public void doSomething(String s1, String s2, String s3)

It's hard to understand what these parameters do without reading the code. Instead:

	public void doSomething(String userName, String userEmail, String userId)

That makes it much easier to understand! Now we'll be able to read the code following the parameter with a much clearer understanding 🙂

### Pronouncable names

When naming fields, methods and classes they should:

- Be readable: Efficient naming means we'll be able to look at the name and understand it instantly, reducing cognitive load on trying to decipher what the name means.

- Be speakable: Names that are speakable avoids awkward conversations where you're trying to pronounce a badly named variable name.

- Be searchable: Nothing is worse than trying to search for a method or variable in a class to realise it's been spelt wrong or badly named. If we're trying to find a method that searches for a user, then searching for 'search' should bring up a result for that method.

- Not use Hungarian notation: Hungarian notation goes against the three points made above, so it should never be used!

### Treat acronyms as words

Any acronyms for class names, variable names etc should be treated as words - this applies for any capitalisation used for any of the letters. For example:

| Do              | Don't           |
|-----------------|-----------------|
| setUserId       | setUserID       |
| String uri      | String URI      |
| int id          | int ID          |
| parseHtml       | parseHTML       |
| generateXmlFile | generateXMLFile |

### Use standard brace style

Braces should always be used on the same line as the code before them. For example, avoid doing this:


    class SomeClass
    {
    	private void someFunction()
    	{
        	if (isSomething)
        	{

        	}
        	else if (!isSomethingElse)
        	{

        	}
        	else
        	{

        	}
    	}
	}

And instead, do this:


	class SomeClass {
    	private void someFunction() {
        	if (isSomething) {

        	} else if (!isSomethingElse) {

        	} else {

        	}
    	}
	}

Not only is the extra line for the space not really necessary, but it makes blocks easier to follow when reading the code.

### Inline if-clauses

Sometimes it makes sense to use a single line for if statements. For example:

    if (user == null) return false;

However, it only works for simple operations. Something like this would be better suited with braces:


    if (user == null) throw new IllegalArgumentExeption("Oops, user object is required.");
    
### Nested if-conditions

Where possible, if-conditions should be combined to avoid over-complicated nesting. For example:

Do:


    if (userSignedIn && userId != null) {

    }

Try to avoid:


    if (userSignedIn) {
        if (userId != null) {

        }
    }

This makes statements easier to read and removes the unnecessary extra lines from the nested clauses.

### Ternary Operators

Where appropriate, ternary operators can be used to simplify operations.

For example, this is easy to read:


    userStatusImage = signedIn ? R.drawable.ic_tick : R.drawable.ic_cross;

and takes up far fewer lines of code than this:


    if (signedIn) {
        userStatusImage = R.drawable.ic_tick;
    } else {
        userStatusImage = R.drawable.ic_cross;
    }

**Note:** There are some times when ternary operators should not be used. If the if-clause logic is complex or a large number of characters then a standard brace style should be used.

## Annotations

### Annotation practices

Taken from  the Android code style guide:

**@Override:** The @Override annotation must be used whenever a method overrides the declaration or implementation from a super-class. For example, if you use the @inheritdocs Javadoc tag, and derive from a class (not an interface), you must also annotate that the method @Overrides the parent class's method.

**@SuppressWarnings:** The @SuppressWarnings annotation should only be used under circumstances where it is impossible to eliminate a warning. If a warning passes this "impossible to eliminate" test, the @SuppressWarnings annotation must be used, so as to ensure that all warnings reflect actual problems in the code.

More information about annotation guidelines can be found here.

----------

Annotations should always be used where possible. For example, using the @Nullable annotation should be used in cases where a field could be expected as null. For example:


    @Nullable 
    TextView userNameText;

    private void getName(@Nullable String name) { }
    
    
### Annotation style

Annotations that are applied to a field, method or class should always be defined in the declaration, with only one per line:

    @Annotation
    @AnotherAnnotation
    public class SomeClass {

      @SomeAnotation
      public String getMeAString() {

      }

    }

    @Bind(R.id.layout_coordinator) 
    CoordinatorLayout coordinatorLayout;

    @Inject 
    MainPresenter mainPresenter;


We do this as it makes the statement easier to read. For example, the statement '@Inject SomeComponent mSomeName' reads as 'inject this component with this name'.

### Limit variable scope

The scope of local variables should be kept to a minimum (Effective Java Item 29). By doing so, you increase the readability and maintainability of your code and reduce the likelihood of error. Each variable should be declared in the innermost block that encloses all uses of the variable.

Local variables should be declared at the point they are first used. Nearly every local variable declaration should contain an initializer. If you don't yet have enough information to initialize a variable sensibly, you should postpone the declaration until you do. - taken from the Android code style guidelines


### Unused elements

All unused **fields**, **imports**, **methods** and **classes** should be removed from the code base unless there is any specific reasoning behind keeping it there.

### Order Import Statements

Because we use Android Studio, so imports should always be ordered automatically. However, in the case that they may not be, then they should be ordered as follows:


1. Android imports
2. Imports from third parties
3. java and javax imports
4. Imports from the current Project

**Note:**

- Imports should be alphabetically ordered within each grouping, with capital letters before lower case letters (e.g. Z before a)
- There should be a blank line between each major grouping (android, com, JUnit, net, org, java, javax)

### Logging

Logging should be used to log useful error messages and/or other information that may be useful during development.


| Log                               | Reason      |
|-----------------------------------|-------------|
| Log.v(String tag, String message) | verbose     |
| Log.d(String tag, String message) | debug       |
| Log.i(String tag, String message) | information |
| Log.w(String tag, String message) | warning     |
| Log.e(String tag, String message) | error       |


We can set the `Tag` for the log as a `static final` field at the top of the class, for example:


    private static final String TAG = MyActivity.class.getName();

All verbose and debug logs must be disabled on release builds. On the other hand - information, warning and error logs should only be kept enabled if deemed necessary.


    if (BuildConfig.DEBUG) {
        Log.d(TAG, "Here's a log message");
    }

**Note:** Timber is the preferred logging method to be used. It handles the tagging for us, which saves us keeping a reference to a TAG.

### Field Ordering

Any fields declared at the top of a class file should be ordered in the following order:

1. Enums
2. Constants
3. Dagger Injected fields
4. Butterknife View Bindings
5. private global variables
6. public global variables

For example:

	public static enum {
		ENUM_ONE, ENUM_TWO
	}

	public static final String KEY_NAME = "KEY_NAME";
	public static final int COUNT_USER = 0;

	@Inject SomeAdapter someAdapter;

	@BindView(R.id.text_name) TextView nameText;
	@BindView(R.id.image_photo) ImageView photoImage;

	private int userCount;
	private String errorMessage;

	public int someCount;
	public String someString;

Using this ordering convention helps to keep field declarations grouped, which increases both the locating of and readability of said fields.

### Class member ordering

There is no single correct solution for this but using a __logical__ and __consistent__ order will improve code learnability and readability. It is recommendable to use the following order:

1. Constants
2. Fields
3. Constructors
4. Override methods and callbacks (public or private)
5. Public methods
6. Private methods
7. Inner classes or interfaces

Example:

```java
public class MainActivity extends Activity {

    private static final String TAG = MainActivity.class.getSimpleName();

    private String mTitle;
    private TextView mTextViewTitle;

    @Override
    public void onCreate() {
        ...
    }

    public void setTitle(String title) {
    	mTitle = title;
    }

    private void setUpView() {
        ...
    }

    static class AnInnerClass {

    }

}
```

If your class is extending an __Android component__ such as an Activity or a Fragment, it is a good practice to order the override methods so that they __match the component's lifecycle__. For example, if you have an Activity that implements `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, then the correct order is:

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle
    @Override
    public void onCreate() {}

    @Override
    public void onResume() {}

    @Override
    public void onPause() {}

    @Override
    public void onDestroy() {}

}
```

### Parameter ordering in methods

When programming for Android, it is quite common to define methods that take a `Context`. If you are writing a method like this, then the __Context__ must be the __first__ parameter.

The opposite case are __callback__ interfaces that should always be the __last__ parameter.

Examples:

```java
// Context always goes first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### String constants, naming, and values

Many elements of the Android SDK such as `SharedPreferences`, `Bundle`, or `Intent` use a key-value pair approach so it's very likely that even for a small app you end up having to write a lot of String constants.

When using one of these components, you __must__ define the keys as a `static final` fields and they should be prefixed as indicated below.

| Element            | Field Name Prefix |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARGUMENT_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |

Note that the arguments of a Fragment - `Fragment.getArguments()` - are also a Bundle. However, because this is a quite common use of Bundles, we define a different prefix for them.

Example:

```java
// Note the value of the field is the same as the name to avoid duplication issues
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String ARGUMENT_USER_ID = "ARGUMENT_USER_ID";

// Intent-related items use full package name as value
static final String EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME";
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

### Arguments in Fragments and Activities

When data is passed into an `Activity` or `Fragment` via an `Intent` or a `Bundle`, the keys for the different values __must__ follow the rules described in the section above.

When an `Activity` or `Fragment` expects arguments, it should provide a `public static` method that facilitates the creation of the relevant `Intent` or `Fragment`.

In the case of Activities the method is usually called `getStartIntent()`:

```java
public static Intent getStartIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra(EXTRA_USER, user);
	return intent;
}
```

For Fragments it is named `newInstance()` and handles the creation of the Fragment with the right arguments:

```java
public static UserFragment newInstance(User user) {
	UserFragment fragment = new UserFragment();
	Bundle args = new Bundle();
	args.putParcelable(ARGUMENT_USER, user);
	fragment.setArguments(args)
	return fragment;
}
```

__Note 1__: These methods should go at the top of the class before `onCreate()`.

__Note 2__: If we provide the methods described above, the keys for extras and arguments should be `private` because there is not need for them to be exposed outside the class.

## Tests

### Espresso Tests or Robolectric Tests

Every Espresso test class usually targets an Activity, therefore the name should match the name of the targeted Activity followed by `Test`, e.g. `SignInActivityTest`

When using the Espresso API it is a common practice to place chained methods in new lines.

```java
onView(withId(R.id.view))
        .perform(scrollTo())
        .check(matches(isDisplayed()))
```
