# Real-time Flight Gate Management System

This system provides a fully functional real-time gate allocation management platform that helps airport operations staff efficiently handle emergencies and automatically optimize gate assignments.

## Features

- **Real-time Flight Information Display**: Visual presentation of all flights and their gate assignments
- **Emergency Response**: Support for handling two common airport emergency situations
  - Gate Closure: When certain gates need to be closed for maintenance or other reasons, the system automatically reassigns affected flights
  - Flight Delays: When flights are delayed, the system considers the new departure times and re-optimizes gate assignments
- **Intelligent Conflict Detection**: Automatically detects and resolves the following conflicts
  - Time Conflicts: Ensures sufficient interval time between flights at the same gate (at least 30 minutes)
  - Gate Restrictions: Ensures flights are not assigned to closed gates
- **Optimization Algorithm**: Uses constraint programming to minimize the distance between flights and their originally assigned gates while resolving conflicts

## System Architecture

- **Frontend**: Responsive user interface built with HTML, CSS, JavaScript, and jQuery
- **Backend**: RESTful API service based on Flask
- **Optimization Engine**: Constraint programming model implemented using Google OR-Tools CP-SAT solver
- **Data Storage**: Excel-based data management

## Installation Guide

### Prerequisites

- Python 3.7+
- pip package manager

### Installing Dependencies

```bash
pip install -r requirements.txt
```

### Data Preparation

The system reads data from the following default paths:
- Flight data: `/aae5103/flight_data_new/flight.xlsx`
- Distance matrix: `/aae5103/flight_data_new/distance.xlsx`

You can also place the data in the project's `data` directory, and the system will search for it automatically.

## Usage Instructions

### Starting the System

```bash
python run.py
```

After startup, visit http://127.0.0.1:8080 to access the system interface.

### System Operation Process

1. **View Current Assignments**: The system automatically loads the current flight and gate assignment information upon startup
2. **Handle Gate Closures**:
   - Select the gates to be closed in the right panel
   - Click the "Close Selected Gates" button
   - The system will automatically trigger reassignment
3. **Handle Flight Delays**:
   - Select the delayed flight in the right panel
   - Enter the new departure time
   - Click the "Update Flight Time" button
   - The system will automatically trigger reassignment
4. **Conflict Detection and Resolution**:
   - The system automatically detects time conflicts in gate assignments
   - If conflicts are found, warnings will be displayed on the interface
   - You can click the "Auto-optimize Assignment" button to resolve conflicts

## Technical Implementation Details

### Conflict Detection Mechanism

The system uses a multi-level conflict detection mechanism:
1. **Frontend Detection**: Detects flight time conflicts through JavaScript on the UI
2. **API Layer Validation**: Validates data format and validity when processing requests
3. **Model Constraints**: Adds hard constraints in the optimization model to ensure no time conflicts

### Optimization Algorithm

Uses a Constraint Programming (CP) model to solve the gate assignment problem:
1. **Decision Variables**: Which gate each flight is assigned to
2. **Constraints**:
   - Each flight must be assigned to one gate
   - Flights at the same gate cannot have time conflicts
   - Flights cannot be assigned to closed gates
3. **Objective Function**: Minimize the total distance between flights and their original gates

## System Characteristics

- **Real-time Response**: The system can complete complex gate reassignments within seconds
- **Automated Processing**: Automatically optimizes assignment plans when conflicts are detected or upon request
- **User-friendly Interface**: Intuitive UI design and clear result display
- **Robustness**: Comprehensive error handling and logging 