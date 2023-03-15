---
permalink: /flight/asynchronous/
---

# Flight Docs

[Back to Flight Docs](/docs/flight/)

## Asynchronous Information

"Asynchronous" refers to something that operates outside of a set timer

The asyncio Python module is used to create concurrent code blocks, that can execute simultaneously. The following functions are used in the flight code:
```
	get_event_loop() : Returns the loop that is currently running all the asynchronous tasks.
	run_until_complete() : Runs the asynchronous task until it finished
```

Futhermore, any functions that are created in a flight file should have a "async" prefix to the function header;
```
	async def extract_gps()
```
This ensures that the function can be run concurrently with other flight functionality.