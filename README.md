# binarysearchtree

A generic **Binary Search Tree (BST)** implemented in Java, with an interactive console menu for inserting, removing, searching, and inspecting values. The tree stores any type that implements `Comparable`, and the driver uses it with integers.

> **Note:** as currently committed, the project does not compile yet. See [Known Issues](#known-issues--work-in-progress) for the exact problems and fixes.

## Features

- Generic tree: `BinarySearchTree<AnyType extends Comparable<? super AnyType>>`
- Insert, remove, and remove-minimum operations
- Find, find-minimum, and find-maximum operations
- Empty-check and clear operations
- Menu-driven `Driver` for trying every operation from the keyboard

## Repository Structure

| File | Description |
| --- | --- |
| `BinaryNode.java` | A tree node holding an element plus `left` and `right` child references. |
| `BinarySearchTree.java` | The BST itself: public operations plus recursive and iterative helper methods. |
| `Driver.java` | Console menu that creates a `BinarySearchTree<Integer>` and calls its operations. |

## Requirements

- Java Development Kit (JDK) 8 or newer

## Build and Run

```bash
javac *.java
java Driver
```

## Usage

The driver shows this menu and repeats it after every action:

```
===================================================
1.Insert 2.Remove 3.RemoveMin 4.FindMin 5.FindMax
6.Find 7.MakeEmpty 8.IsEmpty 9.Print 0.Quit
===================================================
Select an option:
```

### Menu Options

| Option | Operation | Description |
| --- | --- | --- |
| 1 | Insert | Adds an integer to the tree. |
| 2 | Remove | Removes the integer you enter. |
| 3 | RemoveMin | Removes the smallest value in the tree. |
| 4 | FindMin | Shows the smallest value. |
| 5 | FindMax | Shows the largest value. |
| 6 | Find | Reports whether a value is in the tree. |
| 7 | MakeEmpty | Deletes every value from the tree. |
| 8 | IsEmpty | Reports whether the tree has any values. |
| 9 | Print | Intended to print the tree (not implemented yet). |
| 0 | Quit | Intended to exit the program (see Known Issues). |

## How It Works

Each node stores one value. Everything smaller goes into the left subtree and everything larger goes into the right subtree, so a search can ignore half of the remaining tree at each step when the tree is reasonably balanced.

| Operation | Average time | Worst time (unbalanced) |
| --- | --- | --- |
| `insert`, `find`, `remove` | O(log n) | O(n) |
| `findMin`, `findMax`, `removeMin` | O(log n) | O(n) |
| `makeEmpty`, `isEmpty` | O(1) | O(1) |

Removing a node with **two children** replaces its value with the smallest value in its right subtree, then removes that smallest node from the right subtree. A node with one child or none is replaced by that child (or by nothing).

## Known Issues / Work in Progress

**Compile errors**

- `DuplicateItemException` and `ItemNotFoundException` are used in `BinarySearchTree.java` but are not defined anywhere in the repo.
- `print()` calls `height(root)`, which does not exist.
- The private `findMin` ends with `return find(root);`. No `find` method takes a single node, so this does not compile. It should walk down `left` children until `left` is `null`.

**Logic issues**

- **Quit does nothing:** in `Driver`, `case 0: break;` only leaves the `switch`, so the `while(true)` loop never ends. It should call `System.exit(0)`.
- **Duplicates and missing values crash the program:** `insert` throws `DuplicateItemException` for a repeated value, and `remove` and `removeMin` throw `ItemNotFoundException`. The comments say duplicates are ignored and missing values are skipped, and the driver does not catch either exception.
- **Print is empty:** `print()` only computes a height and prints nothing.
- **`findKth` is unfinished:** it loops forever (`while(t.left != null) { count++; }`), returns `root` instead of the matching node, and casts nodes to `BinaryNodeWithSize` even though `insert` only creates plain `BinaryNode` objects (and `size` is never updated).
- **Wrong label:** menu option 5 prints "Minimum is:" for the maximum value.
- **Empty tree:** `findMin()` and `findMax()` return `null` on an empty tree, which prints as `null`.
- **Non-integer input:** the driver reads input with `nextInt()`, so a non-number crashes it.

## License

No license file is included in this repository.
