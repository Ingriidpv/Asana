# Asana Clone — Cross-Platform Task Manager

A cross-platform task management application inspired by Asana, built with .NET MAUI. The app allows multiple users to manage projects and to-do items across macOS, Windows, iOS, and Android from a single shared codebase. Also includes a standalone CLI version of the core functionality.
youtube walkthrough video: https://www.youtube.com/watch?v=Q7u_eJ0yfe0 
---

## Tech Stack

| Layer | Technology |
|---|---|
| UI Framework | .NET MAUI (Multi-platform App UI) |
| Language | C# (.NET) |
| Architecture | MVVM (Model-View-ViewModel) |
| Data binding | INotifyPropertyChanged, ObservableCollection |
| Serialization | System.Text.Json (export/import) |
| CLI interface | .NET Console App |

---

## Features

- **Multi-user support** — switch between users via a dropdown; each user has their own set of projects and to-dos
- **Add / delete projects** — create projects with a name and description; delete them with one tap
- **Add / delete to-dos** — add tasks inline within any project; mark complete with a checkbox or delete individually
- **Real-time completion tracking** — each project displays a live completion percentage that updates automatically when to-dos are checked or removed
- **Search** — filter projects and to-dos in real time by keyword across both project names and task names
- **Sort** — sort the project list by Recently Added, Completion %, or Name A–Z
- **Export / Import** — serialize all projects to JSON and reload them back into the app
- **CLI version** — full CRUD menu for projects and to-dos via the terminal, including listing all to-dos within a given project

---

## Project Structure

```
/
├── Asana.Library/
│   ├── Models/
│   │   ├── ToDo.cs          # Task model with INotifyPropertyChanged
│   │   ├── Project.cs       # Project model with live completion calculation
│   │   └── User.cs          # User model with project list
│   └── Services/
│       ├── UserServiceProxy.cs    # In-memory user store with seed data
│       └── ToDoServiceProxy.cs   # Retrieves projects and todos by user
│
├── Asana.MAUI/
│   ├── MainPage.xaml        # UI layout — search, sort, project cards, todo lists
│   ├── MainPage.xaml.cs     # Event handlers — add, delete, export, import, user switch
│   ├── MainPageViewModel.cs # MVVM ViewModel — state, filtering, sorting, CRUD logic
│   └── Platforms/           # Platform-specific entry points (Android, iOS, macOS, Windows)
│
├── Asana.CLI/
│   └── Program.cs           # Console CRUD menu for projects and to-dos
│
└── COP4870.sln              # Solution file
```

---

## Architecture — MVVM Pattern

The app uses the Model-View-ViewModel pattern to separate concerns:

- **Model** (`Asana.Library`) — plain C# classes (`ToDo`, `Project`, `User`) with property change notification. `Project` automatically recalculates its completion percentage whenever a to-do's `IsCompleted` property changes, using event listeners.
- **ViewModel** (`MainPageViewModel`) — holds all application state in `ObservableCollection<T>` properties. Handles all CRUD operations, search filtering, and sort logic. Notifies the UI of changes via `INotifyPropertyChanged`.
- **View** (`MainPage.xaml`) — declarative XAML UI bound to the ViewModel with two-way data binding. No business logic in the view layer.

---

## Getting Started

### Prerequisites
- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- [Visual Studio 2022](https://visualstudio.microsoft.com/) with the **.NET MAUI** workload installed, or JetBrains Rider

### Run the MAUI app

1. Clone the repository:
   ```bash
   git clone https://github.com/Ingriidpv/Asana.git
   cd Asana
   ```

2. Open `COP4870.sln` in Visual Studio

3. Set `Asana.MAUI` as the startup project

4. Select your target platform (Windows, macOS, Android emulator, or iOS simulator) and run

### Run the CLI app

1. In Visual Studio, set `Asana.CLI` as the startup project and run

   Or from the terminal:
   ```bash
   cd Asana.CLI
   dotnet run
   ```

---

## CLI Menu Options

```
1.  Create a ToDo
2.  List all ToDos
3.  Delete a ToDo
4.  Update a ToDo
5.  Create a Project
6.  Delete a Project
7.  Update a Project
8.  List all Projects
9.  List all ToDos in a given Project
10. Exit
```

---

## Data Model

### Project
| Field | Type | Notes |
|---|---|---|
| Id | int | Auto-incremented |
| Name | string | Project title |
| Description | string | Project details |
| ToDos | List\<ToDo\> | Tasks belonging to this project |
| CompletePercent | double | Auto-calculated from ToDos |

### ToDo
| Field | Type | Notes |
|---|---|---|
| Id | int | Auto-incremented within project |
| Name | string | Task title |
| Description | string | Task details |
| IsCompleted | bool | Triggers completion recalc on change |
| ProjectId | int? | Optional parent project |
| AssignedUserId | string | Username of assigned user |

### User
| Field | Type | Notes |
|---|---|---|
| Id | int | Auto-incremented |
| Username | string | Display name |
| Projects | List\<Project\> | Projects owned by this user |

---

## Export / Import Format

Projects are serialized to JSON and saved to the app's local data directory:

```json
[
  {
    "Id": 1,
    "Name": "Meal Prep",
    "Description": "Prepare the meal prep for this week",
    "CompletePercent": 66.7,
    "ToDos": [
      { "Id": 1, "Name": "Grocery list", "IsCompleted": true },
      { "Id": 2, "Name": "Cook", "IsCompleted": true },
      { "Id": 3, "Name": "Package", "IsCompleted": false }
    ]
  }
]
```

---

## Course

COP4870 — Full-Stack Application Development in C# | Florida State University | Summer 2025
