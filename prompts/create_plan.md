We are going to learn PostgreSQL using Perplexity, ChatGPT, and Warp.

Background:

- perplexity and chatgpt will be used for the longer prompts; setting up the study plan, asking more in depth questions, etc, and to to keep the study in a persistent history
- warp will be used to study the subject in a non-linear fashion, asking questions while trying out things. this is the main idea using ai instead of standard linear text or video tutorials
- our development and study platform is macos
- during development and study, we will run postgresql in docker
- a future project will be hosted with a managed postgresql, so the study plan should exclude things relevant only to a self-hosted postgres setup

Give the reply in a code block for easy copy and paste to our text file.
Commands:

- Come up with a step by step plan for the tutorial, including:

  - Installation and setup
  - psql and pgcli
  - Data types (composite types, jsonb, arrays, ...)
  - User management
  - DDL
  - DML and various joins
  - Indexing and Constraints
  - Transactions & Isolation Levels: MVCC, BEGIN, COMMIT, ROLLBACK, and isolation levels (Read Committed vs. Repeatable Read)
  - CTEs (Common Table Expressions): WITH clauses are standard in modern SQL and make complex queries readable.
  - Window Functions: ROW_NUMBER(), RANK(), LEAD(), LAG() are powerful tools that replace complex self-joins.
  - Performance Basics: Even without deep internals, learning EXPLAIN and EXPLAIN ANALYZE is vital to understand why a query is slow.
  - Explore PostgreSQL’s message queue patterns

- We don't need to study any extensions for now, unless it's important to understand postgresql
- We don't need to go deep, e.g. to learn about b-trees and the like
- We want to use docker-compose
