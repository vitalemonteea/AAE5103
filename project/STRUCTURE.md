# Project Structure and File Organization

This document details the file structure and component functions of the Real-time Flight Gate Management System.

## Core Files and Directories

```
aae5103/project/
├── README.md             # Project overview and usage guide
├── CONTRIBUTING.md       # Code contribution guidelines
├── STRUCTURE.md          # Project structure documentation (this file)
├── requirements.txt      # Project dependencies list
├── run.py                # System startup script
├── app.py                # Web application server
├── gate_assignment_engine.py # Core algorithm engine
├── data/                 # Data files directory
│   ├── flight.xlsx       # Flight data
│   └── distance.xlsx     # Distance matrix
└── templates/            # Web frontend templates
    └── dashboard.html    # Main interface template
```

## Component Functionality

### 1. Entry Script (`run.py`)

Main system entry point, supporting two operation modes: Web interface and command-line interface.
- Configures command-line argument parsing
- Sets up the logging system
- Checks environment and data files
- Launches the appropriate application based on mode

### 2. Web Application Server (`app.py`)

Web application implemented using the Flask framework, providing the following API endpoints:
- `GET /`: Returns the system main interface
- `GET /current_assignment`: Gets the current flight and gate assignment status
- `POST /reassign`: Processes reassignment requests, supporting gate closures and flight delays
- `GET /health`: Health check endpoint for system monitoring

### 3. Core Algorithm Engine (`gate_assignment_engine.py`)

Implements the core algorithms and data processing functions for gate assignment:
- Data loading and preprocessing
- Gate assignment constraint programming model
- Conflict detection and resolution
- Optimization objective calculation

Main functions:
- `load_current_flights()`: Loads current flight data
- `get_current_flights()`: Provides flight information for the API
- `reassign_gates()`: Reassigns gates based on emergency events

### 4. Frontend Interface (`templates/dashboard.html`)

User interface based on HTML, CSS, and JavaScript (jQuery):
- Real-time display of flight and gate assignment status
- Provides functionality for closing gates and setting flight delays
- Triggers reassignment operations and displays results
- Automatically detects and warns of time conflicts

### 5. Data Files (`data/`)

Core data files required by the system:
- `flight.xlsx`: Flight information (flight number, time, status, gate, coordinates)
- `distance.xlsx`: Distance matrix between gates, used to minimize the cost of assignment changes

## Data Flow

1. User performs operations (closing gates or setting flight delays) on the Web interface
2. Frontend sends requests to the server's `/reassign` endpoint
3. Server calls the `reassign_gates()` function in `gate_assignment_engine.py`
4. Algorithm engine loads data, builds an optimization model, and solves for the optimal assignment plan
5. Results are returned to the frontend and the interface display is updated

## Design Patterns and Architecture

The system adopts a classic three-tier architecture:
- **Presentation Layer**: `dashboard.html` provides the user interface
- **Business Logic Layer**: `app.py` and `gate_assignment_engine.py` implement business logic
- **Data Layer**: Excel files provide data storage

Key design decisions:
1. Using Google OR-Tools to implement constraint programming models, efficiently solving complex assignment problems
2. Frontend and backend are completely decoupled, communicating through REST API
3. Global variables `_latest_flights_df` and `_latest_assignment` store the latest assignment results, avoiding frequent file reading

## Improvement Opportunities

Potential system improvements:
1. Introduce a database to replace Excel files, improving data management capabilities
2. Add user authentication and permission management
3. Implement more complex optimization objectives, such as considering passenger connection times
4. Develop monitoring dashboards to display system performance and assignment efficiency metrics 