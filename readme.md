## obsidian으로 이관 예정

[Request]
	•	Migrate all relevant code from the existing ocube-mlops repository that will be used for the second-year development phase.
	•	Clean up the migrated code after migration.

⸻

[Reference]
	•	Update the system flow to the following:
After executing the engine, manage outputs through a modular structure supporting multiple options.
	•	Even if Active Learning is not currently used, manage it as a deactivated module for future potential use.




---

📌 [Request]
	•	Develop a dispatcher module
	•	Route tasks based on job type (e.g., synchronous/asynchronous, CPU/GPU, model type)
	•	Modular branching logic for flexible execution flow
	•	Implement a Celery-based queue worker system
	•	Consume tasks from queue and execute them asynchronously
	•	Track task status, execution logs, and handle retries
	•	Build a worker module to subscribe to model execution requests
	•	Load model → run inference → return result
	•	Support structured task input and output format
	•	Add error handling and retry logic
	•	Handle timeouts, failed tasks, and write logs accordingly
	•	Support resource limitation for tasks
	•	Apply per-task memory and core constraints (e.g., using cgexec, taskset)

⸻

💡 [Reference]
	•	Design should allow potential future extension to other queue systems (e.g., Kafka, DB-backed queues)
	•	Dispatcher should support multiple input types: REST API, CLI, scheduled jobs, etc.
	•	Output modules should support multiple options: save to file, generate graph, return via API
	•	Components like Active Learning or alternative execution modules may be registered as inactive modules for future use
	•	Emphasize clean code structure, test coverage, logging, and exception handling throughout the implementation
