# Laporan Tugas Praktikum Pertemuan 5
## Codelabs: Your first Flutter app

Syahla' Syafiqah Fayra

2141720015

3G/20

## **Steps 3 : Create a Project**

### **Create your first Flutter project**

<img width="960" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/6fa7f1da-ca5b-4699-8b50-82ab58ab2adb">

### **Copy & Paste the initial app**
pubspec.yaml

![image](https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/9d155b9e-7a07-4cc5-aa58-80f6992167ac)

analysis_options.yaml

<img width="500" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/048af6d9-f6e2-4b39-8529-6e513a2dc968">

lib/main.dart
```
import 'package:english_words/english_words.dart';
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => MyAppState(),
      child: MaterialApp(
        title: 'Namer App',
        theme: ThemeData(
          useMaterial3: true,
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
        ),
        home: MyHomePage(),
      ),
    );
  }
}

class MyAppState extends ChangeNotifier {
  var current = WordPair.random();
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();

    return Scaffold(
      body: Column(
        children: [
          Text('A random idea:'),
          Text(appState.current.asLowerCase),
        ],
      ),
    );
  }
}
```
Hasil

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/a3a5086b-5e3a-4664-8fa8-c74ee6c23c9d">

## **Steps 4 : Add a button**

### **Adding a button**
```
return Scaffold(
  body: Column(
    children: [
      Text('A random AWESOME idea:'),
      Text(appState.current.asLowerCase),

      // ↓ Add this.
      ElevatedButton(
        onPressed: () {
          print('button pressed!');
        },
        child: Text('Next'),
      ),

    ],
  ),
);
```
Hasil

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/0488814a-75ef-45a8-80c3-d552e9fa8cf4">

### **A Flutter crash course in 5 minutes**
lib/main.dart
```
void main() {
  runApp(MyApp());
}
```
At the very top of the file, you'll find the main() function. In its current form, it only tells Flutter to run the app defined in MyApp.
```
// ...

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => MyAppState(),
      child: MaterialApp(
        title: 'Namer App',
        theme: ThemeData(
          useMaterial3: true,
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
        ),
        home: MyHomePage(),
      ),
    );
  }
}

// ...
```
The MyApp class extends StatelessWidget. Widgets are the elements from which you build every Flutter app. As you can see, even the app itself is a widget.

The code in MyApp sets up the whole app. It creates the app-wide state (more on this later), names the app, defines the visual theme, and sets the "home" widget—the starting point of your app.

lib/main.dart
```
// ...

class MyAppState extends ChangeNotifier {
  var current = WordPair.random();
}

// ...
```

Next, the MyAppState class defines the app's...well...state. This is your first foray into Flutter, so this codelab will keep it simple and focused. There are many powerful ways to manage app state in Flutter. One of the easiest to explain is ChangeNotifier, the approach taken by this app.
* MyAppState defines the data the app needs to function. Right now, it only contains a single variable with the current random word pair. You will add to this later.
* The state class extends ChangeNotifier, which means that it can notify others about its own changes. For example, if the current word pair changes, some widgets in the app need to know.
* The state is created and provided to the whole app using a ChangeNotifierProvider (see code above in MyApp). This allows any widget in the app to get hold of the state.

lib/main.dart
```
// ...

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {           // ← 1
    var appState = context.watch<MyAppState>();  // ← 2

    return Scaffold(                             // ← 3
      body: Column(                              // ← 4
        children: [
          Text('A random AWESOME idea:'),        // ← 5
          Text(appState.current.asLowerCase),    // ← 6
          ElevatedButton(
            onPressed: () {
              print('button pressed!');
            },
            child: Text('Next'),
          ),
        ],                                       // ← 7
      ),
    );
  }
}

// ...
```

Lastly, there's MyHomePage, the widget you've already modified. Each numbered line below maps to a line-number comment in the code above:

1. Every widget defines a build() method that's automatically called every time the widget's circumstances change so that the widget is always up to date.
2. MyHomePage tracks changes to the app's current state using the watch method.
3. Every build method must return a widget or (more typically) a nested tree of widgets. In this case, the top-level widget is Scaffold. You aren't going to work with Scaffold in this codelab, but it's a helpful widget and is found in the vast majority of real-world Flutter apps.
4. Column is one of the most basic layout widgets in Flutter. It takes any number of children and puts them in a column from top to bottom. By default, the column visually places its children at the top. You'll soon change this so that the column is centered.
5. You changed this Text widget in the first step.
6. This second Text widget takes appState, and accesses the only member of that class, current (which is a WordPair). WordPair provides several helpful getters, such as asPascalCase or asSnakeCase. Here, we use asLowerCase but you can change this now if you prefer one of the alternatives.
7. Notice how Flutter code makes heavy use of trailing commas. This particular comma doesn't need to be here, because children is the last (and also only) member of this particular Column parameter list. Yet it is generally a good idea to use trailing commas: they make adding more members trivial, and they also serve as a hint for Dart's auto-formatter to put a newline there. For more information, see Code formatting.

Next, you'll connect the button to the state.

### **Your first behavior**
Scroll to MyAppState and add a getNext method.

lib/main.dart
```
// ...

class MyAppState extends ChangeNotifier {
  var current = WordPair.random();

  // ↓ Add this.
  void getNext() {
    current = WordPair.random();
    notifyListeners();
  }
}

// ...
```

The new getNext() method reassigns current with a new random WordPair. It also calls notifyListeners()(a method of ChangeNotifier)that ensures that anyone watching MyAppState is notified.

All that remains is to call the getNext method from the button's callback.

lib/main.dart
```
// ...

    ElevatedButton(
      onPressed: () {
        appState.getNext();  // ← This instead of print().
      },
      child: Text('Next'),
    ),

// ...
```
Save and try the app now. It should generate a new random word pair every time you press the Next button.

|Before press the next button |After press the next button |
|-----------------------------|----------------------------|
|<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/0488814a-75ef-45a8-80c3-d552e9fa8cf4"> |<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/55e63244-82a1-4dfe-89ba-fe8c98a305b0"> |

In the next section, you'll make the user interface prettier.

## **Steps 5 : Make the app prettier**
### **Extract a widget**
The line responsible for showing the current word pair looks like this now: Text(appState.current.asLowerCase). To change it into something more complex, it's a good idea to extract this line into a separate widget. Having separate widgets for separate logical parts of your UI is an important way of managing complexity in Flutter.

Flutter provides a refactoring helper for extracting widgets but before you use it, make sure that the line being extracted only accesses what it needs. Right now, the line accesses appState, but really only needs to know what the current word pair is.

For that reason, rewrite the MyHomePage widget as follows:

lib/main.dart
```
// ...

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();  
    var pair = appState.current;                 // ← Add this.

    return Scaffold(
      body: Column(
        children: [
          Text('A random AWESOME idea:'),
          Text(pair.asLowerCase),                // ← Change to this.
          ElevatedButton(
            onPressed: () {
              appState.getNext();
            },
            child: Text('Next'),
          ),
        ],
      ),
    );
  }
}

// ...
```
Nice. The Text widget no longer refers to the whole appState.

Now, call up the Refactor menu. In VS Code, you do this in one of two ways:

1. Right click the piece of code you want to refactor (Text in this case) and select Refactor... from the drop-down menu,

OR

2. Move your cursor to the piece code you want to refactor (Text, in this case), and press Ctrl+. (Win/Linux) or Cmd+. (Mac).

|Before                       |After                       |
|-----------------------------|----------------------------|
|<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/305f413c-a0a0-4471-9d2d-f615f9b7a14b"> |<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/912a4ea7-d5f2-49c2-9e3b-c247b083b530"> |

In the Refactor menu, select Extract Widget. Assign a name, such as BigCard, and click Enter.

This automatically creates a new class, BigCard, at the end of the current file. The class looks something like the following:

lib/main.dart
```
// ...

class BigCard extends StatelessWidget {
  const BigCard({
    super.key,
    required this.pair,
  });

  final WordPair pair;

  @override
  Widget build(BuildContext context) {
    return Text(pair.asLowerCase);
  }
}

// ...
```
<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/8e46bfb4-3efb-4c1e-baaa-d38867ff1331">

Notice how the app keeps working even through this refactoring.

### **Add a Card**
Now it's time to make this new widget into the bold piece of UI we envisioned at the beginning of this section.

Find the BigCard class and the build() method within it. As before, call up the Refactor menu on the Text widget. However, this time you aren't going to extract the widget.

Instead, select Wrap with Padding. This creates a new parent widget around the Text widget called Padding. After saving, you'll see that the random word already has more breathing room.

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/fcbc18d2-178c-432b-8fd7-fabb3dca2be6">

Increase the padding from the default value of 8.0. For example, use something like 20 for roomier padding.

Next, go one level higher. Place your cursor on the Padding widget, pull up the Refactor menu, and select Wrap with widget....

This allows you to specify the parent widget. Type "Card" and press Enter.

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/6e9f7a09-56c3-4788-89a6-35301279eea8">

This wraps the Padding widget, and therefore also the Text, with a Card widget.

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/dac50858-da28-4969-8959-367a4557a983">

### **Theme and style**
To make the card stand out more, paint it with a richer color. And because it's always a good idea to keep a consistent color scheme, use the app's Theme to choose the color.

Make the following changes to BigCard's build() method.

lib/main.dart
```
// ...

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);       // ← Add this.

    return Card(
      color: theme.colorScheme.primary,    // ← And also this.
      child: Padding(
        padding: const EdgeInsets.all(20),
        child: Text(pair.asLowerCase),
      ),
    );
  }

// ...
```
These two new lines do a lot of work:

* First, the code requests the app's current theme with Theme.of(context).
* Then, the code defines the card's color to be the same as the theme's colorScheme property. The color scheme contains many colors, and primary is the most prominent, defining color of the app.

The card is now painted with the app's primary color:
|Before                       |After                       |
|-----------------------------|----------------------------|
|<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/143463e4-e15e-4668-85d8-a17df8f83a33"> |<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/0d0a9e1c-4426-4129-9c6d-e8dc148bab92"> |

You can change this color, and the color scheme of the whole app, by scrolling up to MyApp and changing the seed color for the ColorScheme there.
|Before                       |After                       |
|-----------------------------|----------------------------|
|<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/aa99ffb2-2b64-4a8a-a337-f0780fb0dfd8"> |<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/588343f2-31f9-427a-8d2a-1c8053898f78"> |

Notice how the color animates smoothly. This is called an implicit animation. Many Flutter widgets will smoothly interpolate between values so that the UI doesn't just "jump" between states.

The elevated button below the card also changes color. That's the power of using an app-wide Theme as opposed to hard-coding values.

### **TextTheme**
The card still has a problem: the text is too small and its color is hard to read. To fix this, make the following changes to BigCard's build() method.

lib/main.dart
~~~
// ...

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    // ↓ Add this.
    final style = theme.textTheme.displayMedium!.copyWith(
      color: theme.colorScheme.onPrimary,
    );

    return Card(
      color: theme.colorScheme.primary,
      child: Padding(
        padding: const EdgeInsets.all(20),
        // ↓ Change this line.
        child: Text(pair.asLowerCase, style: style),
      ),
    );
  }

// ...
~~~
What's behind this change:
* By using theme.textTheme, you access the app's font theme. This class includes members such as bodyMedium (for standard text of medium size), caption (for captions of images), or headlineLarge (for large headlines).
* The displayMedium property is a large style meant for display text. The word display is used in the typographic sense here, such as in display typeface. The documentation for displayMedium says that "display styles are reserved for short, important text"—exactly our use case.
* The theme's displayMedium property could theoretically be null. Dart, the programming language in which you're writing this app, is null-safe, so it won't let you call methods of objects that are potentially null. In this case, though, you can use the ! operator ("bang operator") to assure Dart you know what you're doing. (displayMedium is definitely not null in this case. The reason we know this is beyond the scope of this codelab, though.)
* Calling copyWith() on displayMedium returns a copy of the text style with the changes you define. In this case, you're only changing the text's color.
* To get the new color, you once again access the app's theme. The color scheme's onPrimary property defines a color that is a good fit for use on the app's primary color.

The app should now look something like the following:
|Before                       |After                       |
|-----------------------------|----------------------------|
|<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/fe1e8d94-8706-4868-bed7-b9acdfb88329"> |<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/b27b0f80-1286-47eb-992d-3c2e2d0f6c69"> |

If you feel like it, change the card further. Here are some ideas:
* copyWith() lets you change a lot more about the text style than just the color. To get the full list of properties you can change, put your cursor anywhere inside copyWith()'s parentheses, and hit Ctrl+Shift+Space (Win/Linux) or Cmd+Shift+Space (Mac).
* Similarly, you can change more about the Card widget. For example, you can enlarge the card's shadow by increasing the elevation parameter's value.
* Try experimenting with colors. Apart from theme.colorScheme.primary, there's also .secondary, .surface, and a myriad of others. All of these colors have their onPrimary equivalents.

### **Improve accessibility**
lib/main.dart
~~~
// ...

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    final style = theme.textTheme.displayMedium!.copyWith(
      color: theme.colorScheme.onPrimary,
    );

    return Card(
      color: theme.colorScheme.primary,
      child: Padding(
        padding: const EdgeInsets.all(20),

        // ↓ Make the following change.
        child: Text(
          pair.asLowerCase,
          style: style,
          semanticsLabel: "${pair.first} ${pair.second}",
        ),
      ),
    );
  }

// ...
~~~
Now, screen readers correctly pronounce each generated word pair, yet the UI stays the same. Try this in action by using a screen reader on your device.

|Before                       |After                       |
|-----------------------------|----------------------------|
|<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/ce8b5c48-78c2-4e92-a826-c957490d596d"> |<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/0d15187b-c203-4617-bce2-671e0df0f317"> |

### **Center the UI**
Now that the random word pair is presented with enough visual flair, it's time to place it in the center of the app's window/screen.

First, remember that BigCard is part of a Column. By default, columns lump their children to the top, but we can easily override this. Go to MyHomePage's build() method, and make the following change:

lib/main.dart
~~~
// ...

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();
    var pair = appState.current;

    return Scaffold(
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,  // ← Add this.
        children: [
          Text('A random AWESOME idea:'),
          BigCard(pair: pair),
          ElevatedButton(
            onPressed: () {
              appState.getNext();
            },
            child: Text('Next'),
          ),
        ],
      ),
    );
  }
}

// ...
~~~
This centers the children inside the Column along its main (vertical) axis.

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/9a84a518-571a-485c-bf0b-53a3eef17791">

The children are already centered along the column's cross axis (in other words, they are already centered horizontally). But the Column itself isn't centered inside the Scaffold. We can verify this by using the Widget Inspector.

<img width="500" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/57ca02b1-ef6c-48e1-a335-686d027530cd">

The Widget Inspector itself is beyond the scope of this codelab, but you can see that when the Column is highlighted, it doesn't take up the whole width of the app. It only takes up as much horizontal space as its children need.

You can just center the column itself. Put your cursor onto Column, call up the Refactor menu (with Ctrl+. or Cmd+.), and select Wrap with Center.

The app should now look something like the following:

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/72bf058f-fa02-41f5-a86d-0a0ea37a3cf5">


If you want, you can tweak this some more.
* You can remove the Text widget above BigCard. It could be argued that the descriptive text ("A random AWESOME idea:") isn't needed anymore since the UI makes sense even without it. And it's cleaner that way.
* You can also add a SizedBox(height: 10) widget between BigCard and ElevatedButton. This way, there's a bit more separation between the two widgets. The SizedBox widget just takes space and doesn't render anything by itself. It's commonly used to create visual "gaps".

With the optional changes, MyHomePage contains this code:

lib/main.dart
~~~
// ...

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();
    var pair = appState.current;

    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            BigCard(pair: pair),
            SizedBox(height: 10),
            ElevatedButton(
              onPressed: () {
                appState.getNext();
              },
              child: Text('Next'),
            ),
          ],
        ),
      ),
    );
  }
}

// ...
~~~
And the app looks like the following:

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/d0fa6bf7-a324-4e2d-9a7a-df1fb5edbc56">

## **Steps 6 : functionality**
### **Add the business logic**
Scroll to MyAppState and add the following code:
~~~
// ...

class MyAppState extends ChangeNotifier {
  var current = WordPair.random();

  void getNext() {
    current = WordPair.random();
    notifyListeners();
  }

  // ↓ Add the code below.
  var favorites = <WordPair>[];

  void toggleFavorite() {
    if (favorites.contains(current)) {
      favorites.remove(current);
    } else {
      favorites.add(current);
    }
    notifyListeners();
  }
}

// ...
~~~

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/9b43e643-7c97-40f5-b534-b668b756888e">

Examine the changes:
* You added a new property to MyAppState called favorites. This property is initialized with an empty list: [].
* You also specified that the list can only ever contain word pairs: <WordPair>[], using generics. This helps make your app more robust—Dart refuses to even run your app if you try to add anything other than WordPair to it. In turn, you can use the favorites list knowing that there can never be any unwanted objects (like null) hiding in there.
* You also added a new method, toggleFavorite(), which either removes the current word pair from the list of favorites (if it's already there), or adds it (if it isn't there yet). In either case, the code calls notifyListeners(); afterwards.
  
### **Add the button**
With the "business logic" out of the way, it's time to work on the user interface again. Placing the ‘Like' button to the left of the ‘Next' button requires a Row. The Row widget is the horizontal equivalent of Column, which you saw earlier.

First, wrap the existing button in a Row. Go to MyHomePage's build() method, put your cursor on the ElevatedButton, call up the Refactor menu with Ctrl+. or Cmd+., and select Wrap with Row.

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/ebd2c5eb-e1db-443c-9217-46fb0ae5ccd8">

When you save, you'll notice that Row acts similarly to Column—by default, it lumps its children to the left. (Column lumped its children to the top.) To fix this, you could use the same approach as before, but with mainAxisAlignment. However, for didactic (learning) purposes, use mainAxisSize. This tells Row not to take all available horizontal space.

Make the following change:

lib/main.dart
~~~
// ...

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();
    var pair = appState.current;

    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            BigCard(pair: pair),
            SizedBox(height: 10),
            Row(
              mainAxisSize: MainAxisSize.min,   // ← Add this.
              children: [
                ElevatedButton(
                  onPressed: () {
                    appState.getNext();
                  },
                  child: Text('Next'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

// ...
~~~
The UI is back to where it was before.

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/d69a67ad-38fe-4524-948a-476fc5d03dc6">

Here's one way to add the second button to MyHomePage. This time, use the ElevatedButton.icon() constructor to create a button with an icon. And at the top of the build method, choose the appropriate icon depending on whether the current word pair is already in favorites. Also, note the use of SizedBox again, to keep the two buttons a bit apart.

lib/main.dart
~~~
// ...

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();
    var pair = appState.current;

    // ↓ Add this.
    IconData icon;
    if (appState.favorites.contains(pair)) {
      icon = Icons.favorite;
    } else {
      icon = Icons.favorite_border;
    }

    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            BigCard(pair: pair),
            SizedBox(height: 10),
            Row(
              mainAxisSize: MainAxisSize.min,
              children: [

                // ↓ And this.
                ElevatedButton.icon(
                  onPressed: () {
                    appState.toggleFavorite();
                  },
                  icon: Icon(icon),
                  label: Text('Like'),
                ),
                SizedBox(width: 10),

                ElevatedButton(
                  onPressed: () {
                    appState.getNext();
                  },
                  child: Text('Next'),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}

// ...
~~~
The app should look as follows:

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/66d248a9-458f-4a2c-a15e-fb57bb53a4d2">

## **Steps 7 : Add navigation rail**
Select all of MyHomePage, delete it, and replace with the following code:
~~~
// ...

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Row(
        children: [
          SafeArea(
            child: NavigationRail(
              extended: false,
              destinations: [
                NavigationRailDestination(
                  icon: Icon(Icons.home),
                  label: Text('Home'),
                ),
                NavigationRailDestination(
                  icon: Icon(Icons.favorite),
                  label: Text('Favorites'),
                ),
              ],
              selectedIndex: 0,
              onDestinationSelected: (value) {
                print('selected: $value');
              },
            ),
          ),
          Expanded(
            child: Container(
              color: Theme.of(context).colorScheme.primaryContainer,
              child: GeneratorPage(),
            ),
          ),
        ],
      ),
    );
  }
}


class GeneratorPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();
    var pair = appState.current;

    IconData icon;
    if (appState.favorites.contains(pair)) {
      icon = Icons.favorite;
    } else {
      icon = Icons.favorite_border;
    }

    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          BigCard(pair: pair),
          SizedBox(height: 10),
          Row(
            mainAxisSize: MainAxisSize.min,
            children: [
              ElevatedButton.icon(
                onPressed: () {
                  appState.toggleFavorite();
                },
                icon: Icon(icon),
                label: Text('Like'),
              ),
              SizedBox(width: 10),
              ElevatedButton(
                onPressed: () {
                  appState.getNext();
                },
                child: Text('Next'),
              ),
            ],
          ),
        ],
      ),
    );
  }
}

// ...
~~~

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/31d85eb1-f299-4848-8849-6f1a8d8e4822">

### **Stateless versus stateful widgets**
Enter the StatefulWidget, a type of widget that has State. First, convert MyHomePage to a stateful widget.

Place your cursor on the first line of MyHomePage (the one that starts with class MyHomePage...), and call up the Refactor menu using Ctrl+. or Cmd+.. Then, select Convert to StatefulWidget.

<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/88c5142f-c16a-4e69-9fcc-6181774b685c">

### **setState**
The new stateful widget only needs to track one variable: selectedIndex. Make the following 3 changes to _MyHomePageState:

lib/main.dart
~~~
// ...

class _MyHomePageState extends State<MyHomePage> {

  var selectedIndex = 0;     // ← Add this property.

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Row(
        children: [
          SafeArea(
            child: NavigationRail(
              extended: false,
              destinations: [
                NavigationRailDestination(
                  icon: Icon(Icons.home),
                  label: Text('Home'),
                ),
                NavigationRailDestination(
                  icon: Icon(Icons.favorite),
                  label: Text('Favorites'),
                ),
              ],
              selectedIndex: selectedIndex,    // ← Change to this.
              onDestinationSelected: (value) {

                // ↓ Replace print with this.
                setState(() {
                  selectedIndex = value;
                });

              },
            ),
          ),
          Expanded(
            child: Container(
              color: Theme.of(context).colorScheme.primaryContainer,
              child: GeneratorPage(),
            ),
          ),
        ],
      ),
    );
  }
}

// ...
~~~

<img width="241" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/9f54c087-2088-4fd2-ae24-1bad8d7a4117">

The navigation rail now responds to user interaction. But the expanded area on the right stays the same. That's because the code isn't using selectedIndex to determine what screen displays.

### **Use selectedIndex**
Place the following code at the top of _MyHomePageState's build method, just before return Scaffold:

lib/main.dart
~~~
// ...

Widget page;
switch (selectedIndex) {
  case 0:
    page = GeneratorPage();
    break;
  case 1:
    page = Placeholder();
    break;
  default:
    throw UnimplementedError('no widget for $selectedIndex');
}

// ...
~~~

Now that page contains the widget you want to show on the right, you can probably guess what other change is needed.

Here's _MyHomePageState after that single remaining change:

lib/main.dart
~~~
// ...

class _MyHomePageState extends State<MyHomePage> {
  var selectedIndex = 0;

  @override
  Widget build(BuildContext context) {
    Widget page;
    switch (selectedIndex) {
      case 0:
        page = GeneratorPage();
        break;
      case 1:
        page = Placeholder();
        break;
      default:
        throw UnimplementedError('no widget for $selectedIndex');
    }

    return Scaffold(
      body: Row(
        children: [
          SafeArea(
            child: NavigationRail(
              extended: false,
              destinations: [
                NavigationRailDestination(
                  icon: Icon(Icons.home),
                  label: Text('Home'),
                ),
                NavigationRailDestination(
                  icon: Icon(Icons.favorite),
                  label: Text('Favorites'),
                ),
              ],
              selectedIndex: selectedIndex,
              onDestinationSelected: (value) {
                setState(() {
                  selectedIndex = value;
                });
              },
            ),
          ),
          Expanded(
            child: Container(
              color: Theme.of(context).colorScheme.primaryContainer,
              child: page,  // ← Here.
            ),
          ),
        ],
      ),
    );
  }
}


// ...
~~~
The app now switches between our GeneratorPage and the placeholder that will soon become the Favorites page.

<img width="242" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/09525747-775a-48da-96a0-c04812656700">

### **Responsiveness**
Once again, use Flutter's Refactor menu in VS Code to make the required changes. This time, though, it's a little more complicated:
1. Inside _MyHomePageState's build method, put your cursor on Scaffold.
2. Call up the Refactor menu with Ctrl+. (Windows/Linux) or Cmd+. (Mac).
3. Select Wrap with Builder and press Enter.
4. Modify the name of the newly added Builder to LayoutBuilder.
5. Modify the callback parameter list from (context) to (context, constraints).
   
<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/77eb9d1a-2b25-4aa8-903e-41ae2c3d35a4">

Now your code can decide whether to show the label by querying the current constraints. Make the following single-line change to _MyHomePageState's build method:

lib/main.dart
~~~
// ...

class _MyHomePageState extends State<MyHomePage> {
  var selectedIndex = 0;

  @override
  Widget build(BuildContext context) {
    Widget page;
    switch (selectedIndex) {
      case 0:
        page = GeneratorPage();
        break;
      case 1:
        page = Placeholder();
        break;
      default:
        throw UnimplementedError('no widget for $selectedIndex');
    }

    return LayoutBuilder(builder: (context, constraints) {
      return Scaffold(
        body: Row(
          children: [
            SafeArea(
              child: NavigationRail(
                extended: constraints.maxWidth >= 600,  // ← Here.
                destinations: [
                  NavigationRailDestination(
                    icon: Icon(Icons.home),
                    label: Text('Home'),
                  ),
                  NavigationRailDestination(
                    icon: Icon(Icons.favorite),
                    label: Text('Favorites'),
                  ),
                ],
                selectedIndex: selectedIndex,
                onDestinationSelected: (value) {
                  setState(() {
                    selectedIndex = value;
                  });
                },
              ),
            ),
            Expanded(
              child: Container(
                color: Theme.of(context).colorScheme.primaryContainer,
                child: page,
              ),
            ),
          ],
        ),
      );
    });
  }
}


// ...
~~~
Now, your app responds to its environment, such as screen size, orientation, and platform! In other words, it's responsive!.

<img width="467" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/c8e25f6b-cbe3-4169-a114-21bc1768f544">

## **Steps 8 : Add a new page**
What follows is just one way to implement the favorites page. How it's implemented will (hopefully) inspire you to play with the code—improve the UI and make it your own.

Here's the new FavoritesPage class:

lib/main.dart
~~~
// ...

class FavoritesPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    var appState = context.watch<MyAppState>();

    if (appState.favorites.isEmpty) {
      return Center(
        child: Text('No favorites yet.'),
      );
    }

    return ListView(
      children: [
        Padding(
          padding: const EdgeInsets.all(20),
          child: Text('You have '
              '${appState.favorites.length} favorites:'),
        ),
        for (var pair in appState.favorites)
          ListTile(
            leading: Icon(Icons.favorite),
            title: Text(pair.asLowerCase),
          ),
      ],
    );
  }
}
~~~
Here's what the widget does:
* It gets the current state of the app.
* If the list of favorites is empty, it shows a centered message: No favorites yet*.*
* Otherwise, it shows a (scrollable) list.
* The list starts with a summary (for example, You have 5 favorites*.*).
* The code then iterates through all the favorites, and constructs a ListTile widget for each one.

All that remains now is to replace the Placeholder widget with a FavoritesPage. And voilá!
|Awal                         |Like                        |Halaman Like          |
|-----------------------------|----------------------------|----------------------|
|<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/a957ba9e-f339-4fc5-9677-62c4f9c01f80"> |<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/85066e48-625e-4ad4-92e0-221dd2b76839"> |<img width="200" alt="image" src="https://github.com/syahla31/Tugas_Pertemuan_5_Mobile/assets/90373287/b123aa70-c0dd-4596-b3e6-1cb5993d640a"> |
