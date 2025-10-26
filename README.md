# CS-340
SNHU 340


Writing maintainable, readable, adaptable code

On the Grazioso Salvare project, I treated the CRUD Python module as the “single source of truth” for all database access. Key practices:

Separation of concerns: Dash callbacks never touched MongoDB directly; they called AnimalShelterCRUD methods (e.g., read, create). That kept the UI clean and the data layer swappable.

Clear interfaces & docstrings: Each method had a narrow purpose, typed params/returns, and docstrings, so later changes didn’t ripple through the codebase.

Input validation & safe queries: Centralizing filtering, projections, and pagination reduced injection risk and made queries predictable.

Logging & error handling: The module logged query shapes and raised domain-specific errors; debugging became straightforward.

Testability: I could unit-test CRUD in isolation with fixtures and a test DB, then integration-test the dashboard on top.

Advantages: Faster iteration, easier debugging, less duplicate code in callbacks, and the ability to profile and optimize queries (e.g., adding indexes) in one place.

Future use:

Reuse the module in a CLI tool for data audits, an ETL job to clean/import new shelter data, or a small FastAPI/Flask microservice to expose the same operations to other apps.

Schedule maintenance tasks (archiving old records) or drive analytics notebooks that share the same query functions.

My problem-solving approach (for the DB + dashboard)

Start from user stories: “As a coordinator, I need to filter dogs by breed/age/sex and see them on a map.”

Model the data: Decide fields, normalize where needed, and add indexes on high-traffic filters (e.g., animal_type, breed, age_upon_outcome_in_weeks, sex_upon_outcome).

Design the API surface first: Write CRUD method signatures and expected outputs; then wire Dash callbacks to those methods.

Iterate with real data: Use sampled AAC records to verify filters, projections, and map markers.

Quality gates: Linting (flake8/black), docstrings, unit tests on CRUD, and quick performance checks on common queries.

How this differed from earlier coursework: Instead of one-off scripts, this was client-facing, iterative, and layered (data, service, UI). I had to think about maintainability (indexes, error messages, empty-state UI), not just “make it work.”

Techniques I’ll reuse on future client DBs:

Schema contracts with Pydantic/Marshmallow; migrations/versioning of data shape.

Environment-based configs (.env) and role-based access (read vs admin).

Pre-commit hooks (format/lint/tests) and lightweight CI.

Caching hotspots and adding targeted indexes based on logs.

Containerizing (Docker) for reproducible dev/prod parity.

What computer scientists do—and why it matters

Computer scientists design abstractions and systems that turn messy, real-world needs into reliable, testable, and evolvable software. On a project like Grazioso Salvare’s:

We reduce operational friction: staff can query exactly the animals that meet rescue criteria in seconds instead of combing spreadsheets.

We improve decisions: consistent filtering + geospatial views (map) reveal patterns (e.g., intake clusters, breed availability).

We scale safely: centralized CRUD, validation, and access control mean more teammates and tools can plug in without breaking data quality.

For Grazioso, that translates to quicker matching of dogs to missions, better deployment planning, and measurable KPIs (time-to-find, placement rates). In short, disciplined engineering practices make their lifesaving work faster, clearer, and more dependable.
