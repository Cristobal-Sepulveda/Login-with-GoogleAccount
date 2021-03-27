



________________________________________________________________________________

                          1. Introduction
________________________________________________________________________________

Reference Documentation
    * Firebase Authentication
https://firebase.google.com/docs/auth
    * FirebaseUI for Auth
https://github.com/firebase/FirebaseUI-Android/blob/master/auth/README.md

________________________________________________________________________________


________________________________________________________________________________

                            2. Getting Started
________________________________________________________________________________

Download the Code for this Lesson

  You have two options for the code in this lesson:
      * You can download a zip file of the starter code here

      * If you’re familiar with git, you can clone this repository. The master
        branch of the repository contains the solution code up to
        Concept 8: Summary & Next Exercise and the ‘start’ branch of the
        repository contains //TODO comments that you will implement as you
        work through the lesson. As you begin this lesson, make sure you are
        on the ‘start’ branch of the repository.
https://github.com/udacity/android-kotlin-login/tree/start

Add Firebase to the project

Step 1: Create a Firebase project

  Before you can add Firebase to your Android app, you need to create a
  Firebase project to connect to your Android app.

    1. In the Firebase console, click Add project, then select or enter a
       Project name. You can name your project anything, but try to pick a
       name relevant to the app you’re building.
https://console.firebase.google.com/u/0/

    2. Click Continue.

    3. You can skip setting up Google Analytics and chose the Not Right
       Now option.

    4. Click Create Project to finish setting up the Firebase project.

Step 2: Register your app with Firebase

  Now that you have a Firebase project, you can add your Android app to it.

    1. In the center of the Firebase console's project overview page, click the
       Android icon to launch the setup workflow.

    2. Enter your app's application ID in the Android package name field. Make
       sure you enter the ID your app is using, otherwise you cannot add or
       modify this value after you’ve registered your app with your Firebase
       project.

      * An application ID is sometimes referred to as a package name.

      * Find this application ID in your module (app-level) Gradle file, usually
        app/build.gradle (example ID: com.yourcompany.yourproject).

    3. Enter the debug signing certificate SHA-1. You can generate this key by
       entering the following command in your command line terminal

            keytool -alias androiddebugkey -keystore ~/.android/debug.keystore -list -v -storepass android

    4. Click to Register app.

Step 3: Add the Firebase configuration file to your project

  Add the Firebase Android configuration file to your app:

    1. Click Download google-services.json to obtain your Firebase Android
       config file (google-services.json).

       * You can download your Firebase Android config file again at any time.

       * Make sure the config file is not appended with additional characters
         and should only be named google-services.json.

    2. Move your config file into the module (app-level) directory of your app.

Step 4: Configure your Android project to enable Firebase products

    1. To enable Firebase products in your app, add the google-services plugin
       to your Gradle files.

       a) In your root-level (project-level) Gradle file (build.gradle), add
          rules to include the Google Services plugin. Check that you have
          Google's Maven repository, as well.

                build.gradle
                buildscript {

                  repositories {
                    // Check that you have the following line (if not, add it):
                    google()  // Google's Maven repository
                  }

                  dependencies {
                    // ...

                    // Add the following line:
                    classpath 'com.google.gms:google-services:4.3.0'  // Google Services plugin
                  }
                }

                allprojects {
                  // ...

                  repositories {
                    // Check that you have the following line (if not, add it):
                    google()  // Google's Maven repository
                    // ...
                  }
                }

      b) In your module (app-level) Gradle file (usually app/build.gradle),
         add a line to the bottom of the file.

                      app/build.gradle

                      apply plugin: 'com.android.application'

                      android {
                        // ...
                      }
                    // Add the following line to the bottom of the file:
                    apply plugin: 'com.google.gms.google-services'  // Google Play services Gradle plugin

Add the Firebase dependency

  In this lesson the main use case for integrating Firebase is to have a way
  to create and manage users. So far you’ve set up your app so that it can
  communicate with Firebase, but now you need to add a specific Firebase
  library that enables you to implement login.

//TODO 1.0
    1. The firebase-auth SDK allows management of authenticated users of your application. Add the following dependency in your build.gradle (Module:app)
    file so that you can use the SDK in your app.

          app/build.gradle
          implementation 'com.firebaseui:firebase-ui-auth:5.0.0'

    2. Sync your project with gradle files.

  To be sure that all dependencies are available to your app, you should sync your project with gradle files. Select File > Sync Project with Gradle Files in Android Studio, or from the toolbar.

Run the app

  Once you’ve set up Firebase, run the app on an emulator or physical device to
  ensure that your environment has successfully been set up to start
  development. If successful, you should see the home screen display a fun
  Android fact and a login button on the top left corner. Tapping the
  login button doesn’t do anything just yet.


________________________________________________________________________________


________________________________________________________________________________

                          3. Project Walkthrough
________________________________________________________________________________

                                NOTHING HERE
                                NOTHING HERE
                                NOTHING HERE

________________________________________________________________________________


________________________________________________________________________________

                      4. Enabling Authentication Methods
________________________________________________________________________________


Enable Authentication Methods

  In this step you’ll use the Firebase Console to set up the authentication
  methods you want your app to support. For this lesson, you will focus on
  letting users login with an email address they provide or their
  Google account.

    1. Navigate to the Firebase console and select your project if you are not there already.
    https://console.firebase.google.com/u/0/

    2. Select Develop > Authentication on the left side panel.

    3. Select the Sign In Method tab on the top navigation bar.

    4. Click on the Email/Password row and toggle the ‘Enabled’ switch and click Save.

    5. Click on the Google row, toggle the Enabled switch, enter a Project
       support email, and click Save.

QUIZ QUESTION

  When providing users with different options to login, how many options should you provide?

Just a few so the user is not overwhelmed with options

________________________________________________________________________________


________________________________________________________________________________

                        5. Enabling Login
________________________________________________________________________________

In this step you’ll implement the login feature for your users.

`TODO 1.1`
    1. Open the file MainFragment.kt. In the MainFragment’s layout there is an auth_button, but it’s currently not setup to handle any user input yet. In onViewCreated(), add an onClickListener to auth_button to call launchSignInFlow().

      MainFragment.kt
            binding.authButton.setOnClickListener { launchSignInFlow() }

`TODO 1.2`
    2. Look for the launchSignInFlow() method in MainFragment.kt that currently contains a TODO. Complete the launchSignInFlow() function by allowing users to register and sign in with their email address or Google account. If the user chooses to register with their email address, the email and password combination they create is unique for your app. That means they will be able to login to your app with the email address and password combination, but it doesn’t mean they can also login to any other Firebase supported app by default.

      MainFragment.kt
              private fun launchSignInFlow() {
                 // Give users the option to sign in / register with their email or Google account.
                 // If users choose to register with their email,
                 // they will need to create a password as well.
                 val providers = arrayListOf(
                     AuthUI.IdpConfig.EmailBuilder().build(), AuthUI.IdpConfig.GoogleBuilder().build()

                     // This is where you can provide more ways for users to register and
                     // sign in.
                 )

                 // Create and launch sign-in intent.
                 // We listen to the response of this activity with the
                 // SIGN_IN_REQUEST_CODE
                 startActivityForResult(
                     AuthUI.getInstance()
                         .createSignInIntentBuilder()
                         .setAvailableProviders(providers)
                         .build(),
                     MainFragment.SIGN_IN_REQUEST_CODE
                 )
              }

`TODO 1.3`
    3. In MainFragment.kt, you can listen for the result of the sign-in process
       by implementing the onActivityResult() method. Since you started the
       sign in process with SIGN_IN_REQUEST_CODE, you can also listen to the
       result of the sign in process by filtering for when SIGN_IN_REQUEST_CODE
       is passed back to onActivityResult(). Start by having some log
       statements to know whether the user has signed in successfully.

      MainFragment.kt
            override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
               super.onActivityResult(requestCode, resultCode, data)
               if (requestCode == SIGN_IN_REQUEST_CODE) {
                   val response = IdpResponse.fromResultIntent(data)
                   if (resultCode == Activity.RESULT_OK) {
                       // User successfully signed in
                       Log.i(TAG, "Successfully signed in user ${FirebaseAuth.getInstance().currentUser?.displayName}!")
                   } else {
                       // Sign in failed. If response is null the user canceled the
                       // sign-in flow using the back button. Otherwise check
                       // response.getError().getErrorCode() and handle the error.
                       Log.i(TAG, "Sign in unsuccessful ${response?.error?.errorCode}")
                   }
               }
            }

    4. Your app should now be able to handle registering and logging in users!
       Run the app and verify that tapping on the Login button now brings up
       the login screen. You can now sign in with your email address and a
       password, or with your Google account. There will be no change in the
       UI after you login (you’ll implement updating the UI in the next step),
       but if everything is working correctly, you should see the log message Successfully signed in user ${your name}! after you go through the
       registration flow. You can also go into the Firebase console and
       navigate to Develop > Authentication > Users to check that the app now
       has one registered user.

________________________________________________________________________________


________________________________________________________________________________

                      6. Updating UI - Introduction
________________________________________________________________________________

                              NOTHING HERE
                              NOTHING HERE
                              NOTHING HERE
________________________________________________________________________________  


________________________________________________________________________________

                      7. Updating UI - Code
________________________________________________________________________________


  In this step, you’ll implement updating the UI based on the authentication
  state. When the user is logged in, you can personalize their home screen
  by displaying their name. You will also update the Login button to be a
  Logout button when the user is logged in.

    1. The first thing you need to do is provide a way for other classes in
       the app to know when a user has logged in or out. For this purpose, the FirebaseUserLiveData.kt class is already created for you.
       However, the class doesn’t do anything yet as the value of the
       LiveData isn’t being updated.

      Since you are using the FirebaseAuth library, you can listen to changes to
      the logged in user with the FirebaseUser. AuthStateListener callback that’s
      implemented for you as part of the FirebaseUI library. This callback will get
      triggered whenever a user logs in or out of your app.

      Notice that FirebaseUserLiveData.kt defines the authStateListener variable.
      You will use this variable to store the value of the LiveData. The
      authStateListener variable was created so that you can properly start and
      stop listening to changes in the auth state based on the state of your
      application. For example, if the user puts the app into the background, then
      the app should stop listening to auth state changes in order to prevent
      any potential memory leaks.

      Update authStateListener so that the value of your FirebaseUserLiveData
      corresponds to the current Firebase user.

`TODO: 2.1`
          FirebaseUserLiveData.kt
              private val authStateListener = FirebaseAuth.AuthStateListener { firebaseAuth ->
                 value = firebaseAuth.currentUser
              }

`TODO: 2.2`
    2. Next, in LoginViewModel.kt, create an authenticationState variable based
       off of the FirebaseUserLiveData object you just implemented. By creating
       this authenticationState variable, other classes can now query for whether the user is logged in or not through the LoginViewModel.

       LoginViewModel.kt
              val authenticationState = FirebaseUserLiveData().map { user ->
                 if (user != null) {
                     AuthenticationState.AUTHENTICATED
                 } else {
                     AuthenticationState.UNAUTHENTICATED
                 }
              }

`TODO: 2.3`
    3. Now in MainFragment.kt’s observeAuthenticationState() you can use the authenticationState variable you just added in LoginViewModel and change the UI accordingly. If there is a logged-in user, authButton should display Logout.

      MainFragment.kt
              private fun observeAuthenticationState() {
                 val factToDisplay = viewModel.getFactToDisplay(requireContext())

                 viewModel.authenticationState.observe(viewLifecycleOwner, Observer { authenticationState ->
                     when (authenticationState) {
                         LoginViewModel.AuthenticationState.AUTHENTICATED -> {
                             binding.authButton.text = getString(R.string.logout_button_text)
                             binding.authButton.setOnClickListener {
                                 // TODO implement logging out user in next step
                             }

                              // TODO 2. If the user is logged in,
                               // you can customize the welcome message they see by
                               // utilizing the getFactWithPersonalization() function provided

                         }
                         else -> {
                             // TODO 3. Lastly, if there is no logged-in user,
                              // auth_button should display Login and
                              //  launch the sign in screen when clicked.
                         }
                     }
                 })
              }

    4. If the user is logged in, you can customize the welcome message they see by utilizing the getFactWithPersonalization() function provided in MainFragment.

      MainFragment.kt
              binding.welcomeText.text = getFactWithPersonalization(factToDisplay)

    5. Lastly, if there is no logged-in user (when authenticationState is
       anything other than LoginViewModel.AuthenticationState.AUTHENTICATED),
       auth_button should display “Login” and launch the sign in screen
       when clicked. There should also be no personalization of the
       message displayed.

    MainFragment.kt
              binding.authButton.text = getString(R.string.login_button_text)
              binding.authButton.setOnClickListener { launchSignInFlow() }
              binding.welcomeText.text = factToDisplay

    6. With all the steps completed, your final observeAuthenticationState() method should look similar to the example shown below.

    MainFragment.kt
              private fun observeAuthenticationState() {
                 val factToDisplay = viewModel.getFactToDisplay(requireContext())

                 viewModel.authenticationState.observe(viewLifecycleOwner, Observer { authenticationState ->
                      // TODO 1. Use the authenticationState variable you just added
                       // in LoginViewModel and change the UI accordingly.
                     when (authenticationState) {
                          // TODO 2.  If the user is logged in,
                           // you can customize the welcome message they see by
                           // utilizing the getFactWithPersonalization() function provided
                         LoginViewModel.AuthenticationState.AUTHENTICATED -> {
                             binding.welcomeText.text = getFactWithPersonalization(factToDisplay)
                             binding.authButton.text = getString(R.string.logout_button_text)
                             binding.authButton.setOnClickListener {
                                 // TODO implement logging out user in next step
                             }
                         }
                         else -> {
                              // TODO 3. Lastly, if there is no logged-in user,
                               // auth_button should display Login and
                               // launch the sign in screen when clicked.
                             binding.welcomeText.text = factToDisplay

                             binding.authButton.text = getString(R.string.login_button_text)
                             binding.authButton.setOnClickListener {
                                 launchSignInFlow()
                             }
                         }
                     }
                 })
              }

    7. Now the UI should update according to whether a user is logged in or not.
       Run the app again. If everything is working properly, the home screen
       should now greet you by your name in addition to displaying an Android
       fact. The Login button should also now display Logout.

    8. Since the app allows users to login, it should also provide them with a
       way to logout. Here’s an example of how to log out a user with just one
       line of code:

            AuthUI.getInstance().signOut(requireContext())

  In MainFragment.kt’s observeAuthenticationState(), add the logout logic so that the auth_button functions correctly when there is a logged in user.

________________________________________________________________________________


________________________________________________________________________________

                        8. Summary & Next Exercises
________________________________________________________________________________

  If you want to take a look at the solution up till this point, you can check
  out this Github repo.
  https://github.com/udacity/android-kotlin-login

________________________________________________________________________________


________________________________________________________________________________

                  9. Condition Navigation - Introduction
________________________________________________________________________________

                            NOTHING HERE
                            NOTHING HERE
                            NOTHING HERE
________________________________________________________________________________

________________________________________________________________________________

                    10. Enable Settings Button
________________________________________________________________________________

  In this step, you will add a button on the home screen that allows the user
  to navigate to the settings screen. The settings screen will let the user
  pick what kind of fun fact they want displayed on the home screen. From the
  settings screen, they can either choose to see facts about Android or facts
  about the state of California.

`TODO 3.1`
    1. In fragment_main.xml, add a Settings button nested in the
       ConstraintLayout and position it at the top right corner of the screen.

    fragment_main.xml
            <TextView
                   android:id="@+id/settings_btn"
                   android:layout_width="wrap_content"
                   android:layout_height="wrap_content"
                   android:layout_margin="@dimen/text_margin"
                   android:background="@color/colorAccent"
                   android:padding="10dp"
                   android:text="@string/settings_btn"
                   android:textColor="#ffffff"
                   android:textSize="20sp"
                   app:layout_constraintRight_toRightOf="parent"
                   app:layout_constraintTop_toTopOf="parent"/>

`TODO 3.2`
    2. In nav_graph.xml, add an action inside mainFragment. The id of the
       action is action_mainFragment_to_customizeFragment and the destination
       is customizeFragment.

    nav_graph.xml
              <fragment
                     android:id="@+id/mainFragment"
                     android:name="com.example.android.firebaseui_login_sample.MainFragment"
                     android:label="MainFragment">
                 <action
                         android:id="@+id/action_mainFragment_to_settingsFragment"
                         app:destination="@id/settingsFragment"/>
              </fragment>

`TODO 3.3`
    3. In MainFragment.kt’s onViewCreated(), set an onClickListener for
       settings_btn so that tapping the button will navigate the user to customizeFragment. Don’t worry if you see unresolved errors as you
       implementation action! If you see unresolved errors, you can recompile
       the app from the Build menu to generate and use the new navigation
       actions you just created in the xml.

    MainFragment.kt
              binding.settingsBtn.setOnClickListener {
                 val action = MainFragmentDirections.actionMainFragmentToSettingsFragment()
                 findNavController().navigate(action)
              }

    4. Recompile and relaunch the app If everything went well, there should now
       be a functional Settings button on the top right corner. Clicking on the
       button should take you to the Settings screen, and click the back button
       of the Android device should bring you back to the home screen. The Settings screen only has one option to let the user choose what type of fun fact they want to see displayed on the home screen.
;

________________________________________________________________________________

________________________________________________________________________________

                    11. Navigation Based on State
________________________________________________________________________________

  If the user is not logged in, it doesn’t make much sense for them to be
  able to set any customizations. In this step, you will add the code to
  navigate the user to the login screen if they attempt to access the
  settings screen when they are not logged in.

`TODO 3.4`
    1. In SettingsFragment.kt’s onViewCreated(), observe the
       authenticationState and redirect the user to LoginFragment
       if they are not authenticated.

    SettingsFragment.kt
            override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
               super.onViewCreated(view, savedInstanceState)
               val navController = findNavController()
               viewModel.authenticationState.observe(viewLifecycleOwner, Observer { authenticationState ->
                   when (authenticationState) {
                       LoginViewModel.AuthenticationState.AUTHENTICATED -> Log.i(TAG, "Authenticated")
                       // If the user is not logged in, they should not be able to set any preferences,
                       // so navigate them to the login fragment
                       LoginViewModel.AuthenticationState.UNAUTHENTICATED -> navController.navigate(
                           R.id.loginFragment
                       )
                       else -> Log.e(
                           TAG, "New $authenticationState state that doesn't require any UI change"
                       )
                   }
               })
            }

`TODO 3.5`
    2. Since the app takes the user to the login screen if they try to
       access the settings screen, the app also needs to handle the back
       button behavior on the login screen. If the app doesn’t customize
       handling the back button behavior, the user would be stuck in an
       infinite loop of trying to go back to the Settings screen, but
       then be redirected to the login screen again.

       In LoginFragment.kt’s onViewCreated(), handle back button actions by bringing the user back to the MainFragment.

    LoginFragment.kt
            // If the user presses the back button, bring them back to the home screen
            requireActivity().onBackPressedDispatcher.addCallback(viewLifecycleOwner) {
               navController.popBackStack(R.id.mainFragment, false)
            }

    3. Relaunch your app and confirm that if you’re not logged in, attempts
       to access the Settings screen will now redirect you to the login flow. While you have successfully redirected the user to login, you haven’t handled what happens after the user successfully logs in, so attempts
       to login will appear is if it’s not working. You will fix this in the
       next step.
________________________________________________________________________________


________________________________________________________________________________

                          12. Navigation After Login
________________________________________________________________________________

  So far, you’ve successfully set up your app to redirect the user to the login screen if they try to access the Settings screen without being logged in.

  However, you might have noticed that after you go through the sign in flow, you are brought back to the Login screen again. This is not a good user experience and can be confusing to the user.

  To provide the ideal user experience, the app should bring the user back to the Settings screen after the user successfully logs in.

`TODO 3.6`
    1. In LoginFragment.kt’s onViewCreated(), observe the authenticationState and navigate the user back to SettingsFragment when they are successfully authenticated.

    LoginFragment.kt
            // Observe the authentication state so we can know if the user has logged in successfully.
            // If the user has logged in successfully, bring them back to the settings screen.
            // If the user did not log in successfully, display an error message.
            viewModel.authenticationState.observe(viewLifecycleOwner, Observer { authenticationState ->
               when (authenticationState) {
                   LoginViewModel.AuthenticationState.AUTHENTICATED -> navController.popBackStack()
                   else -> Log.e(
                       TAG,
                       "Authentication state that doesn't require any UI change $authenticationState"
                   )
               }
            })

    2. Run your app again and confirm that now when you successfully sign in, you land on the Settings page instead of the Login page.


Popping the Back Stack

  As the user progresses through an app, Android keeps track of the screens they visit. When the user presses the back button, the app goes back through the visited screens in order.

  When you pop a screen off the backstack, you are basically telling Android to skip that screen when the user presses the back button.

  In this case, you are removing the current screen from the back stack so that when the user presses the back button, the app skips this screen and goes back to the screen that the user visited before visiting this one.

  In other words, when the user presses the back button, the app does not come back to the login screen, instead it goes to the screen that the user visited before coming to the login screen.

________________________________________________________________________________


________________________________________________________________________________

                                13. Quiz
________________________________________________________________________________

QUESTION 1 OF 3
Applications should avoid providing users with alternate authentication options if a first-party option exists.


False

QUESTION 2 OF 3
Applications should allow access to application features if a user is not authenticated.

True


QUESTION 3 OF 3
It is bad practice to change the UI of the behavior of a feature based on the authentication state.


False


________________________________________________________________________________


________________________________________________________________________________

                            14. Final Summary
________________________________________________________________________________


  In this lesson you allowed users to register and sign in with their email address.

  To check out the solutions for the entire lesson, you can visit this Github repo.
https://github.com/udacity/android-kotlin-login-navigation

  With the FirebaseUI library you can also support other authentication methods such as signing in with a phone number. To learn more about the capabilities of the FirebaseUI library and how to utilize other functionalities it provides, check out the following resources:

    * FirebaseUI’s Authentication Documentation
    https://firebase.google.com/docs/auth/android/firebaseui
    * FirebaseUI Authentication Demo and Sample Code
    https://github.com/firebase/FirebaseUI-Android/blob/master/auth/README.md

For more about best practices around login, check out these other resources:

  Codelabs:

    * Optimize your app for autofill
https://codelabs.developers.google.com/codelabs/optimize-autofill/index.html?index=..%2F..index#0
    * Seamless Sign In with Smart Lock
https://codelabs.developers.google.com/codelabs/android-smart-lock/#0

Android developer documentation:

    * Android’s Autofill Framework
https://developer.android.com/guide/topics/text/autofill
    * Smart Lock for Passwords on Android
https://developers.google.com/identity/smartlock-passwords/android/

Videos:

    * Optimizing User Flows through Login
    https://www.youtube.com/watch?v=dbMxvTG0KEU&ab_channel=AndroidDevelopers
