# Course Repository for T-488-MAPP
## By Edda Steinunn Rúnarsdóttir
#### Includes course weekly projects under ./Projects (except the Xamarin Forms project which resides at [this GitHub repository](https://github.com/eddast/T-488-MAPP-Xamarin-Forms)) and some code done in class from lecture material under ./Lecture Codes

## Week 1: Xamarin.iOS project - Visual Demonstration and Information
In the first week of the course T-488-MAPP an Xamarin.iOS project was implemented in C# via Visual Studio. The project's function was built throughout one week, with some new functionalities every day.  Git was used as a version control tool throughout the week, hence this repository contains change history of this project throughout this entire week, making prior functionalities that may have been omitted available.

### Application Structure and Purpose
The application is a tabbed application with two tabs and it fetches information about movies from an external web service and displays that information appropriately. Each tab contains a view with it's own navigation controller.

### First tab: Search
The first tab is the tab user is automatically navigated to once app launches. The first tab prompts user to write in a query substring of some movie, then displays a button on which the user can click. Once clicked, the tab becomes un-clickable - both literally and visually - and a load spinner appears to indicate background process. The screen remains in this state while resources (movie information) are retrieved from the server:

![alt text](https://image.ibb.co/j6NvSw/One.jpg)

Once view has loaded, user is navigated to the next screen (a view is placed on the navigation stack) which is a table view structure displaying all movies that matched user query string (of course, if a query string is empty or no results were found, this view is an empty table view). Each non-empty cell is clickable and navigates user to another view (placed on the navigation stack) which displays images and details of movie which have already been fetched in the load screen before displaying the table view:

![alt text](https://image.ibb.co/izKmZb/Two.jpg)

### Second tab: Top Rated

This tab loads top rated movies from the external web service and initially displays a load spinner indicating background process. Once resources have been retrieved, the top rated movies are displayed in the same table view as the once that displays the search results but with one major difference - this view is not placed on the navigation stack like it does when search is conducted, but replaces the loading view. The table cells are clickable like in the search view and once clicked show movie details. Every time a user navigates from this tab to another, and then back into this tab to the root view (table view), the information is retrieved again meanwhile the same load screen with a load spinner is displayed:

![alt text](https://image.ibb.co/m3BB0G/Three.jpg)

### Known Limitations

Due to lack of time and too specified service model of the external web service's API, the potential cross-platform shareable code of this model could not be placed into the portable class library. This was because the image downloader service (located in the MovieDownload namespace) was included in this service model and could not reside in the PCL due to platform-specific references. I will attempt to separate the image downloader service from the API service model and solve this before starting week two's Android project. _(Edit: Fixed 03.12.17)_

## Week 2: Xamarin.Droid project - Visual Demonstration and Information
Note that images displaying app's functionality in this section are from tests conducted on an actual android phone (Samsung Galaxy Alpha) and not by an emulator. Also note that the app's minimum software requirement is Android 4.0.3, namely API level 15 but it's target version is Android with API level 26.

### Application Structure and Purpose
The application has essentially the same function as week one's iOS app with minor changes: again a tabbed application was created with two tabs whose purpose is to fetch information about movies from an external web service and displays that information appropriately. Toolbar was used to generate the tab bar function and fragments were loaded into each tabs by a main fragment activity.

### First tab: Search
The first tab is the tab user is automatically navigated to once app launches. The first tab prompts user to write in a query substring of some movie, then displays a button on which the user can click. Once clicked, the tab becomes un-clickable - both literally and visually - and a load spinner appears to indicate background process. The screen remains in this state while resources (movie information) are retrieved from the server:

![alt text](https://image.ibb.co/ier69b/Search_View.jpg)

Once resources have been retrieved, user is navigated to the next screen (i.e. a new activity starts on top of the fragment activity which can be retracted via back button present on android phones) which includes a list view displaying all movies that matched user query string. Each item (movie) in that list is clickable and navigates user to another screen (again, starts a new activity) which displays images and details of that movie:

![alt text](https://image.ibb.co/fgqYpb/ListView.jpg)

### Second tab: Top Rated
This tab loads top rated movies from the external web service and initially displays a load spinner indicating background process. Once resources have been retrieved, an empty list view this fragment contains is filled with movies retrieved. The list items are clickable like in the search view and once clicked show movie details. Every time a user navigates into the top rated tab the information is retrieved again, displaying the fragment's load spinner appropriately:

![alt text](https://image.ibb.co/dNmdNw/Top_Rated_Tab.jpg)

### Some enhancements from the iOS app
Four major enhancements emerge in the android app: the method of retrieving images, the user interface, the user experience and code sharing.

- **Glide** was now used to retrieve images (movie posters and movie backdrop images) which increases efficiency greatly as it asynchronously retrieves images as activity is displayed on screen, thus reducing loading time a bit.

- The **User Experience** is much better in the android app than in the iOS app. In addition to naming the app **AMDb** for **App Movie DataBase** a "logo" for the app was designed. Moreover a fixed colour palette was used for the overall look of the app and a launch screen was provided, conforming to the palette and including the logo. These changes made a an obvious difference in the app appearance from the raw and default look of the iOS app. See launch screen:

![alt text](https://image.ibb.co/hqxnXw/launchscreen.jpg)

- The **User Interface** was greatly enhanced. Now error messages are displayed when user inputs no string and when user receives no results. Also, user can now click on an information icon in the initial screen of the app for information on it's functionality:
![alt text](https://image.ibb.co/mCH4Cw/UIexp1.jpg)
![alt text](https://image.ibb.co/cJKZCw/UIexp2.jpg)
![alt text](https://image.ibb.co/i19Ueb/UIexp3.jpg)

- In addition to this to enhance the UI, now **average rating** of a movie is now displayed below title in the form of stars and numeric rate. This is because most potential users that got to try the app when in progress found it imperative to be able to get some idea of the movie quality in movie inspection based apps like mine. Since displaying the average rating required no extra time complexity nor too much work this was added to the detail view for a better user experience.

- This week's app provides a better **Code sharing**. First of all the known limitation for the iOS app was fixed (service not being in the portable class library). In addition to this, this week's project provides a base for a better MovieModel object in the PCL. This is because the iOS version of the MovieModel depended greatly on a MovieInfo object it took in which I realised made my MovieModel over-bloated and less efficient. Therefore, the MovieModel now isolates all factors it needs from the MovieInfo object it takes in and uses those factors in isolation when needed. The android portion of the project never depends on the MovieModel's MovieInfo property. Please note that although my intention was to entirely remove the MovieInfo property from the MovieModel class, it remains due to the iOS part of the project needing to compile.

### Known Limitations
I thought it would be relatively easy renaming namespaces and project files of a project in Visual Studio and therefore I left that for last. Little did I know. After hours of googling I deem this task impossible given the time I have before the project is due. Therefore my android project of week two has the unfortunate name of "IOSWeek1". Also, the app can crash at random because of communication issues between either the app and the external web service or between the web service and the movie database; for example if there is too much load on the server (this results in a somehow malformed response from server) or if there is no internet connection. But since teacher stated that this would not be accounted for when grading these cases are not explicitly handled due to limited time for the project.

## Week 3: Xamarin Forms project - Visual Demonstration and Information
Note that this project is not stored in this repository, but at https://github.com/eddast/T-488-MAPP-Xamarin-Forms. Further Note that images displaying app in this section are from tests conducted on an iPhone iOS 11 Simulator for iOS,
a Samsung Galaxy Alpha Mobile Phone (Android 5.0 with API 21) for Android and a Lenovo ThinkPad Computer for UWP.

### Application Structure and Purpose
The application has partially the same function as week one and two's iOS and Android app,
again a tabbed application was created but now with three tabs whose purpose is to fetch information about movies
from an external web service and display that information appropriately.
This week's app however differs from the others because it is cross-platform compatible and fetches information
significantly faster than the other apps since it uses "lazy-load" (i.e. loads after rendering page/screen)
for some properties of movie information.

### First tab: Movie Search
The first tab is the tab user is automatically navigated to once app launches.
The first tab prompts user to write in a query substring of some movie into a search bar, then once user hits enter button on keyboard
a load spinner (generally very briefly) appears to indicate background process while resources are fetched from server. Once resources have been fetched,
A list appears in the view displaying results from user's query.
Some elements of the movie list is fetched asynchroniously like the poster and the movie cast displayed in each cell and therefore manifest later than the title and year.
The list of movies can be scrolled through.
Each item in list can be clicked and navigates user to a detail page displaying detail on the clicked movie.
Below are images that demonstrate the Search tab's functionality:

![alt text](https://image.ibb.co/cU9mZm/Search_Tab1.jpg)
![alt text](https://image.ibb.co/cdE9n6/Search_Tab2.jpg)

### Second tab: Top Rated Movies
This tab loads top rated movies from the external web service. The information is fetched when app launches (more specifically, when the first tab page has been rendered)
and initially displays a load spinner indicating background process, although in practise user will not see this as he will not be able to navigate to this tab quickly enough,
since the list is fetched quite quickly in general. Once resources have been retrieved, list is displayed. The list items are clickable like in the search view and once clicked navigate to another page showing movie details.
This list has a pull-to-refresh function, where when user pulls the list down, the list is fetched and rendered again in the view, thus updating any potential outdated data.
Below are images that demonstrate the Top Rated Movies tab's functionality:

![alt text](https://image.ibb.co/f8oDEm/Top_Rated_Tab.jpg)
(Note that the first frame demonstrates the pull-to-refresh function; since as stated before some resources are asynchroniously fetched like images and cast members and these things have not been fetched in this frame for all cells as reloading is in progress. This is intentional to demonstrate the "lazy-load")

### Third tab: Popular Movies
This tab has essentially the same functionality as the Top Rated Movies tab, except that movies displayed in this view's list
are movies that are popular at the moment instead of displaying all-time top rated movies. That is, list is fetched and can be pulled to refresh and list items are clickable and navigate to another page showing movie details.
Below are images that demonstrate the Popular Movies tab's functionality:

![alt text](https://image.ibb.co/eoZ3Em/Popular_Tab.jpg)

### The Movie Detail View
The detail view for some movie is accessible from every movie list present in the app. Every item in movie list in app is clickable
and navigates user to the movie detail view where basic information such as title and overview are instantly displayed while other information like images, runtime and tagline are fetched asynchroniously after this view has rendered.
Note that the detail view is very structually different from the structure we were supposed to implement by the project description.
This was because of attempting to achieve consistency in app appearance from the previous week's apps. Nontheless, the view displays all it is supposed to display.
The entire view can be scrolled (i.e. is inside a scrollview) and in addition to that, the movie overview can be scrolled too if it's size exceeds the height of the poster image beside it.
See images describing this functionality below:

![alt text](https://image.ibb.co/k9HZLR/Detail_View.jpg)
(In the first frame, the tagline and runtime have not yet been fetched to demonstrate the view's lazy load. Then after a brief period this is fetched and the end results is as shown in the second frame. The third and the fourth frame display possible scrolling of the view and the overview section)

### Cross-Platform Compatibility
All function demonstrated above is preserved for both Android and UWP (except that UWP does not support the pull-to-load functionality). Both are quite usable and have similar user interface. Below are visual demonstrations of these two remaining platforms although these images only show the appearance of the two platforms, since functionality has already been demonstrated in details.

#### Android

The Android Search tab appearance (after searching for a query string):
![alt text](https://image.ibb.co/nCD5um/Android_Search.jpg)

The Android Top Rated tab appearance:
![alt text](https://image.ibb.co/eNngZm/Android_Top_Rated.jpg)

The Android Popular tab appearance:
![alt text](https://image.ibb.co/h2VDfR/Android_Popular.jpg)

#### UWP

The UWP Search tab appearance (after searching for a query string):
![alt text](https://image.ibb.co/bsD5um/UWPSearch.jpg)

The UWP Top Rated tab appearance:
![alt text](https://image.ibb.co/n6Oc76/UWPTop_Rated.jpg)

The UWP Popular tab appearance:
![alt text](https://image.ibb.co/jgWx76/UWPPopular.jpg)

#### Custom Rendering
Very minimal custom rendering is present in this project because that was considered unnecessary since all platforms display the information quite delightfully. Although, the tab bar displayed on iOS is custom rendered and displays icons to enhance user experience because without it the appearence seemed unnatural.

### Enhancements from previous iOS and Android apps
Various changes were made in functionality, coding convention and code structure but one enhancement was above all those.
As previously stated, this week's Xamarin Forms application is significantly faster than the other apps created for the course as some information is fetched asynchroniously ("lazy-loaded"), such as movie images, movie cast, movie tagline and movie runtime. This does not greatly affect the user experience because even though page does not display full information initially, it takes little time fetching the rest of the information. To put this a bit into context, a minor experiment was conducted; recieveing results with "Fargo" as a query string on last week's Android app took 12.29 seconds, while on the Xamarin Forms app it takes a mere half a second (0.28s) to receive results! (Both tests were conducted on Samsung Galaxy Alpha). Moreover judging by this minor experiment, performance was enhanced by 98% in this week's app versus last week's app. (performance represented as percentage of speed increment for this specific experiment)

### Known limitations
The app uses significant memory due to many tasks running on various threads when resources are fetched which sometimes can cause problems on memory-sensitive devices. For example, my phone is four years old (meaning ancient in phone-years) and sometimes crashes when a lot of information is fetched at once in the app. To make the application even able to run on my phone, I was forced to specify a large memory heap for the app's memory usage in the Android Manifest code. However, I conducted tests on other better and newer phones which had no problem at all running the app however the user navigated on it and used it. In addition to this, the app can crash at random because of communication issues between either the app and the external web service or between the web service and the movie database; for example if there is too much load on the server (this results in a somehow malformed response from server) or if there is no internet connection. But since teacher stated that this would not be accounted for when grading these cases are not explicitly handled due to
limited time for the project.
