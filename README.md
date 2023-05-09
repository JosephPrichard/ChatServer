# ChatServer
Simple web server to handle chat rooms with websockets. The web server uses Axum's websocket libraries to communicate over websockets. Since Axum uses Tokio as an async runtime, the server is able to handle concurrent web socket connections. Each chat room is stored in a DashMap (concurrency safe sharded hash map) including a chat log, and the last time the room was accessed. Since multiple connections can access a room at the same time, the rooms themseleves are wrapped inside an arc mutex and accessing room state involves locking the mutex. Each room contains a broadcast channel that is used to broadcast messages to all connections. The server uses a background task to prune expired entries from the dashmap every hour. The background task locks each individual room entry but never the entire DashMap, ensuring the rooms stay available to provide high performance.

## About
This was written to practice networks in Rust and to get a better understanding of async and concurrency in Rust. I learned a TON about async/await, concurrency, and structures such as mutexes and channels.
