Where2Skate 🛹
An Android application for discovering, sharing, and rating skateparks and street spots. 

[!IMPORTANT]
This application requires a Firebase project setup and a valid Google Maps API key to function correctly. Please ensure all prerequisites in the setup guide are met before building and running the project.

What is Where2Skate?
Where2Skate is a mobile application built with Kotlin for the Android platform, designed to be the ultimate companion for skateboarders. Its core purpose is to create a community-driven map of the best places to skate, from official skateparks to local street spots.

The application allows users to:

Securely register and log in to a personal account.

Discover new skate spots through an interactive map powered by Google Maps.

Contribute to the community by adding new, undiscovered locations.

Rate and review existing spots, helping others find the best places to skate.

Connect with other skaters through a shared platform of locations and feedback.

Key Features
Firebase Authentication: Secure user registration and login using email and password, managed by Firebase Auth.

Interactive Spot Map: Utilizes the Google Maps SDK for Android to display all user-submitted skate spots as markers on a map, providing an intuitive way to explore.

User-Generated Spots (CRUD): Registered users can perform Create, Read, Update, and Delete (CRUD) operations for skate spots, making the app's database a living, community-curated resource.

Rating & Review System: A five-star rating system allows users to score spots and leave comments, providing valuable feedback for the community.

Real-time Database: Built on Cloud Firestore, ensuring all spot information and ratings are updated and synced across all users' devices in real-time.

Modern Android Tech Stack: Developed using Kotlin, Jetpack Compose for a declarative and modern UI, and follows recommended Android architecture patterns.

Application Screens
This section will showcase the main user interfaces of the application.

(You can add your screenshots here later)

Login & Registration Screen: Clean and simple interface for user authentication.

Home/Map Screen: The main screen featuring the interactive map with all skate spots.

Spot Details Screen: A view showing detailed information about a specific spot, including its name, description, photos, and user ratings.

Add/Edit Spot Screen: A form for users to submit new spots or edit existing ones, including a map interface to pinpoint the exact location.

Development 🧑‍🔧
Prerequisites
Android Studio: Hedgehog (2023.1.1) or a newer version.

Kotlin: The project is configured to use Kotlin.

Google Maps API Key: A valid API key with the Maps SDK for Android enabled.

Firebase Account: A Firebase project is required to handle authentication and the database.

Quickstart Guide ⚡️
Follow these steps to get the project running on your local machine or an emulator.

Clone the repository

# Replace the URL with your repository's URL
$ git clone https://your-repo-url/where2skate.git
$ cd where2skate

Open in Android Studio

Open the cloned project folder in Android Studio. Gradle will automatically start syncing the project dependencies.

Set up Firebase

Go to the Firebase Console and create a new project.

Add a new Android app to the project, using com.example.where2skate as the package name.

Follow the setup steps to download the google-services.json file.

Place the downloaded google-services.json file into the app/ directory of your project.

In the Firebase Console, enable Authentication (with the Email/Password provider) and Cloud Firestore.

Add Google Maps API Key

Go to the Google Cloud Console and get your API Key.

Make sure the Maps SDK for Android is enabled for your key.

Open the app/src/main/AndroidManifest.xml file.

Find the line with com.google.android.geo.API_KEY and replace "YOUR_API_KEY" with your actual key.

<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="AIzaSy...YOUR_REAL_API_KEY" />

Build and Run the Application

Select a target device (emulator or physical device) and click the "Run" button in Android Studio. The application should build and install.

Firebase Firestore Rules
For the application to work correctly, you need to set up security rules for your Firestore database. Go to your Firebase project -> Firestore Database -> Rules and paste the following:

rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {

    // Skateparks can be read by anyone, but only created/updated/deleted by logged-in users.
    match /skateparks/{skateparkId} {
      allow read: if true;
      allow create, update, delete: if request.auth != null;
    }

    // Ratings can be read by anyone.
    // A user can only create a rating if they are logged in.
    // A user can only update/delete their own rating.
    match /ratings/{ratingId} {
      allow read: if true;
      allow create: if request.auth != null;
      allow update, delete: if request.auth != null && resource.data.userId == request.auth.uid;
    }
  }
}
