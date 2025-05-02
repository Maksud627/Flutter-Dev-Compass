- everything is widget in flutter.
- Widgets describe **how UI should look** (but they don't actually draw it yet).
- Widgets are immutable (once created, they cannot change)
- (If you want something to change, you create a new widget.)

```dart
@override
Widget build(BuildContext context) {
  return Column(
    children: [
      Text('Hello World'),
      ElevatedButton(onPressed: () {}, child: Text('Click Me'))
    ],
  );
}

// widget tree will be built like this
Column
 ├── Text ('Hello World')
 └── ElevatedButton
      └── Text ('Click Me')
```
- each widget is a node in a tree

- ***Rendering pipeline***
    
    ```css
    Widgets → Elements → RenderObjects → Painting → Compositing → Display
    ```
- ***Widget + Rendering pipeline***
    
    
    | Step | What happens | Key Classes |
    | --- | --- | --- |
    | **1. Widget** | Blueprint describing UI | `Widget` classes |
    | **2. Element** | **Live instances** that hold the ***widget's state*** and ***location*** in the tree | `Element` classes (like `StatelessElement`, `StatefulElement`) |
    | **3. RenderObject** | Real **object responsible for layout and painting** | `RenderObject` classes |
    | **4. Painting** | Drawing operations on the screen | Canvas |
    | **5. Compositing** | Layers combined and sent to GPU | `Layer` classes |
    
    Note: 👉 *Flutter rebuilds the widget tree when setState() is called, but not necessarily the whole screen!*
    
    ### 1. **Widgets** (Blueprints)
    
    - Lightweight descriptions of the UI.
    - Created in `build()` method.
    - Immutable.
    
    > 👉 Flutter rebuilds the widget tree when setState() is called, but not necessarily the whole screen!
    > 
    
    ---
    
    ### 2. **Elements** (Instantiated Widgets)
    
    - When a widget is inserted into the tree, **an Element is created**.
    - Element maintains the **relationship between widgets and render objects**.
    - Two main types of elements:
        - `StatelessElement`
        - `StatefulElement`
    
    > 👉 Elements manage the lifecycle of widgets.
    > 
    
    ---
    
    ### 3. **RenderObjects** (Layout and Painting)
    
    - RenderObjects are responsible for:
        - **Measuring** how big widgets should be (layout)
        - **Painting** the widget onto the screen
    - Complex classes like `RenderBox`, `RenderParagraph`, etc.
    
    > 👉 Heavy lifting happens here.
    > 
    
    ---
    
    ### 4. **Painting**
    
    - Each RenderObject **paints** itself on a **Canvas**.
    - Only dirty areas (changed parts) are repainted — that's why Flutter is super fast!
    
    ---
    
    ### 5. **Compositing**
    
    - Flutter **splits the scene into layers**.
    - Sends the layers to the **GPU** for faster rendering.
    
    Every frame (screen refresh), Flutter does these:
    
    1. **Build Phase**
        - Runs `build()` methods → updates Widget Tree.
    2. **Layout Phase**
        - RenderObjects figure out **size and position**.
        - Constraints (like min/max sizes) are passed down.
    3. **Paint Phase**
        - RenderObjects draw themselves onto the screen.
    4. **Compositing Phase**
        - Layers combined and sent to screen.
    
    ## 🧠 Pro Tips:
    
    - **setState()** doesn't rebuild everything — it just rebuilds the part of the tree that changed.
    - **Keys** (GlobalKey, ValueKey) help Flutter **identify** widgets between rebuilds (important for performance).
    - **Stateless vs Stateful Widgets**:
        - Stateless → no dynamic data
        - Stateful → has internal data that changes
- ***Widget and rendering pipeline Example***
    
    sequences →
    
    - When button is pressed → `setState()` → triggers `build()`
    - New widget tree is created
    - Flutter **compares old and new trees** (this is called the **Element diffing**)
    - Only updates what actually changed (efficient!).
    
    ```dart
    class CounterApp extends StatefulWidget {
      @override
      _CounterAppState createState() => _CounterAppState();
    }
    
    class _CounterAppState extends State<CounterApp> {
      int _counter = 0;
    
      void _increment() {
        setState(() { 
          _counter++;
        });
      }
    
      @override
      Widget build(BuildContext context) {
        return Column(
          children: [
            Text('Count: $_counter'),
            ElevatedButton(onPressed: _increment, child: Text('Increment'))
          ],
        );
      }
    }
    
    ```