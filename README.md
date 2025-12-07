Campus Communications System (CCS) 🎓

The Campus Communications System (CCS) is a robust, hybrid networking application built in C++ that facilitates real-time, inter-departmental, and inter-campus communication using a strategic combination of TCP for reliability and UDP for speed.

This system is designed around a central server (simulating the Islamabad campus) that handles client authentication, message routing, file transfer, and network monitoring for satellite campuses (e.g., Lahore, Karachi).

✨ Features

Hybrid Protocol Architecture

TCP (Port 5000): Used for client authentication, reliable messaging, and file transfer to ensure data integrity and ordered delivery.

UDP (Port 6000 / 6001): Used for lightweight, non-critical tasks like sending heartbeats (client → server) and instant admin broadcasts (server → all clients).

Concurrency:
The server employs a Thread-per-Client model using std::thread to handle multiple simultaneous connections without blocking. Critical shared data (client sockets, inboxes) are protected using std::mutex to prevent race conditions.

Message and File Routing:
Messages and files are routed based on both target campus and target department (e.g., Lahore|Admissions).

Offline Message Queue:
Messages destined for offline campuses are temporarily stored in the server's inbox and can be manually forwarded by the administrator upon reconnection.

Real-time Monitoring:
The Admin Console provides live tools (list and hbshow) to monitor connected clients and their most recent UDP heartbeat status.

🛠️ Usage & Setup
Requirements

A C++11 compatible compiler (e.g., g++)

POSIX networking libraries (standard on Linux/macOS)

The -pthread flag is required for compiling the multithreaded components

Compilation
# Compile the Server executable
g++ -std=c++11 server.cpp -o server -pthread

# Compile the Client executable
g++ -std=c++11 campus_client.cpp -o campus_client -pthread

Running the System

Start the Server:

./server


The server will start listening and provide an [ADMIN] command prompt.

Start Clients:
Open new terminal windows for each campus client you wish to run.

./campus_client


You will be prompted to enter a Campus Name (e.g., Lahore, Karachi) and the corresponding Password.

Admin Commands (Server Console)
Command	Description
list	Shows currently connected campuses and their last TCP activity.
hbshow	Shows all campuses that sent a UDP heartbeat and the timestamp.
broadcast <message>	Sends a UDP broadcast message instantly to all active clients.
inbox	Lists messages stored for the Islamabad campus or offline clients.
`open <C>	<D>`
