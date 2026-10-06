# NOTES

## What I changed
- **TaskRepository:** I added brackets in the SQL query. Without them, archived tasks and wrong statuses came in the search results.
- **TaskController:** I removed the sleep delay that made searches slow. I also added a try/catch for a wrong status (now it gives a 400 error, not a 500), and I fixed page and pageSize so 0 or negative numbers don't crash it.
- **App.jsx:** The page now goes back to 1 when the search or the status changes. I also made one PAGE_SIZE value instead of writing 10 in two places.
- **useTasks.js:** Loading now stops even when the request fails, the old error is cleared, and old responses are ignored. I added a 300 ms delay (debounce) for typing in the search box.

## What I did not change
- Pagination is still done in memory. Changing it to Pageable needs more changes in the API, so I left it.
- Task status is still a String and not an enum in the entity, because schema.sql and data.sql already use it.
- I kept the native SQL and did not move to JPQL, for the same reason.
- I did not add DTOs, a real logger or AbortController. They are good ideas, but they are not needed to fix the bugs.

## Biggest risk
Search loads all matching rows into memory, and LIKE '%word%' can't use an index. It is fine for around 50 rows, but it will be slow when the data grows. Also there are no tests, so a future change can break these fixes without anyone knowing.

## Tools/AI used
I used Claude as a reviewer. I first ran the application and tested it manually for around 5 minutes and I found a few bugs in the application. Next I went through the code manually and I found a bug in backend and I tried to debug the issue myself and I found a few bugs in the code. I pasted some files and asked it to find errors, and asked for the root cause and fix. I have done all my previous projects in Python so I am a Java beginner but I have good experience with React, so I also asked it to explain catch and the page clamping until I understood them. I applied all the fixes myself.