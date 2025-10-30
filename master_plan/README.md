# Practical 1 :  Basic State with Model-View

### 1. Complete the lab steps, then document the final results with a GIF and explanation in the file README.md! If you find any errors or issues, please fix them.

<p align="center">
  <img src="img\all done.gif" width="250" alt="1" />
</p>
In this practicum, the data is stored directly inside the PlanScreen using a local variable (plan).
Each time the user adds a new task, the list is updated within the local state using setState().

**Final Result**

The app can add new tasks using the FloatingActionButton and display the list of tasks within a single plan.
* All data is still stored within a single screen (local state).
* After the app is restarted, all data will be lost because no separate data management system has been implemented yet.
* There is also no separation between the model and view layers at this stage.

### 2. Explain the purpose of step 4 in the practicum! Why is this done?
This step is done to define the data model that represents a single task in the master plan — for example, each learning goal or to-do item. By creating this class (e.g., class Plan or class Task), the app can:
* Store the name and status (done/not done) of each plan item.
* Manage the list of plans efficiently (add, update, delete).
* Separate the data (model) from the UI (view), following the MVC or MVVM principle in Flutter.

### 3. Why is the plan variable needed in step 6 of the lab? Why is it a constant?
The `plan` variable contains the entire list of `Task` or `Plan` objects that represent the user’s master plan.

```dart
final List<Plan> plan = [];
```

**Function**

This variable is used to store all the plan items that users create.  
It allows the UI to display the list of plans and update their states (**checked** or **unchecked**) dynamically when users interact with the app.

**Why It’s Declared as final**

Declaring the variable as `final` means the reference to the list itself cannot be changed, but its contents can still be modified (such as adding or removing plans).

This provides two key advantages:
1. It prevents the list reference from being accidentally reassigned.  
2. It keeps the internal data structure stable while still allowing modifications to its contents.

In short, the list remains constant in memory, but the data inside it can evolve based on user actions.

### 4. Capture the results of Step 9 as a GIF, then explain what you have created!

<p align="center">
  <img src="img\step 9.gif" width="250" alt="1" />
</p>

The GIF demonstrates the working Master Plan app. The user can view tasks, mark them as completed, and add new tasks in real time using Flutter’s stateful widget system.

### 5. What is the use of the methods in Steps 11 and 13 in the lifecycle state ?
* initState() can be used to load existing plans or initialize variables.
* dispose() is used to properly dispose of the TextEditingController to prevent memory leaks.

### 6. Submit your practicum report in the form of a commit link or GitHub repository to the agreed lecturer!
----

# Practical 2 :  InheritedWidget

### 1. Complete the lab steps, then document the final results with a GIF and explanation in the file README.md! If you find any errors or issues, please fix them.

### 2. Explain what is meant InheritedWidgetby step 1! Why is it used InheritedNotifier?
**InheritedWidget**
InheritedWidget is a fundamental Flutter widget that allows data or state to be shared across the entire widget subtree without manually passing it through constructors every time.
It is commonly used for simple state management.
Widgets below it in the tree can listen to data changes from the InheritedWidget and automatically rebuild when the data updates. However, a regular InheritedWidget rebuilds the entire widget tree whenever data changes, which is inefficient for frequently updated data.

**InheritedNotifier**
InheritedNotifier is a subclass of InheritedWidget combined with ChangeNotifier.
It rebuilds only the widgets that depend on the notifier when a change occurs.
This makes it much more efficient for data that changes often, such as counters, lists, or form inputs.

### 3. Explain the purpose of the method in step 3 of the practicum! Why is this done?
The purpose of this method is to make it easier for other widgets to access data stored in the InheritedWidget without needing to create a new instance.

The of(context) function acts as a global access point (like a global getter) to the shared data within the widget tree.

This method allows other widgets to easily access the data provided by the InheritedWidget through the BuildContext, and automatically rebuild when the shared data changes.

### 4. Capture the results of Step 9 as a GIF, then explain what you have created!
<p align="center">
  <img src="img\prac 2.gif" width="250" alt="1" />
</p>

The final application displays a counter value that can be increased or decreased using buttons. The counter state is managed using an InheritedNotifier, which efficiently updates only the widgets that depend on the counter value. This demonstrates how InheritedNotifier can be used for reactive and lightweight state management without external packages.


### 5. Submit your practicum report in the form of a commit link or GitHub repository to the agreed lecturer!
----

# Practical 3 :   State in Multiple Screens

### 1. Complete the lab steps, then document the final results with a GIF and explanation in the file README.md! If you find any errors or issues, please fix them.


### 2. Based on the Practical 3 that you have done, explain the meaning of the following diagram!
The diagram illustrates the widget hierarchy and state flow in the Master Plan application after the state was lifted up using an InheritedWidget (specifically InheritedNotifier), so it can be shared across multiple screens. This diagram demonstrates how Flutter efficiently manages global state across multiple screens using InheritedNotifier, without depending on external state management libraries such as Provider or Riverpod.

**Main Page (PlanCreatorScreen)**

* MaterialApp
The root widget of the Flutter app that handles themes, navigation, and routes between screens.

* PlanProvider
An InheritedNotifier that holds the global state as a List<Plan>.
All widgets under it can access and modify the plan list without manually passing data through constructors (avoiding prop drilling).

* PlanCreatorScreen
The first page that displays all existing plans and allows the user to add new ones.

* Column
Arranges widgets vertically. Contains a TextField and a plan list (Expanded → ListView).

* TextField
Input field used to create a new plan.

* Expanded → ListView
Displays all existing plans.
When a plan is tapped, it triggers navigation using Navigator.push() to open the detail page (PlanScreen).

**Navigator Push**
The “Navigator Push” arrow in the middle of the diagram represents navigation between screens, from PlanCreatorScreen to PlanScreen.
The selected plan data is passed through the constructor PlanScreen(plan: plan).

**Plan Detail Page (PlanScreen)**

* MaterialApp
Still within the same app context.

* PlanScreen
Displays detailed information of a selected plan, including its task list.

* Scaffold
Provides the main layout of the page, including AppBar, body, and FloatingActionButton.

* Column
Arranges the content vertically, showing the task list and the progress summary below.

* Expanded → ListView
Contains the list of tasks that can be checked or edited using Checkbox and TextFormField.

* SafeArea → Text
Displays the completion status (e.g., “3 out of 5 tasks complete”) at the bottom of the screen.

### 3. Capture the results of Step 14 as a GIF, then explain what you have created!
<p align="center">
  <img src="img\prac 3.gif" width="250" alt="1" />
</p>

The purpose of this practicum is to apply the **Lift State Up** concept so that data can be accessed and shared across multiple screens. It uses an **InheritedNotifier** (`PlanProvider`) to store the global `List<Plan>` and connect different pages seamlessly. In this implementation, two main screens are used: **PlanCreatorScreen**, which displays all plans and allows users to add new ones, and **PlanScreen**, which shows the details and task list of the selected plan. Navigation between these screens is handled using `Navigator.push()`, passing the selected plan as an argument so that both screens share the same state.


### 4. Submit your practicum report in the form of a commit link or GitHub repository to the agreed lecturer!