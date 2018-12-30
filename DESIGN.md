
# Design

pg_phrase is a PostgreSQL extension that does automatic phrase extraction.  This document describes the design of the extension, how it is expected to be used, and motivation and implementation of design choices.

## Desired Usage

```sql
-- Creates the extension in schema pg_phrase.  Phrase models can be separated by creating the extension in different schemas.
create extension pg_phrase;

-- Specify fields that should be analyzed for learning phrases.
insert into pg_phrase.analyzed_text_fields 
    (table, text_column, group_column, time_column) 
values 
    ('reviews', 'body', 'company_name', 'created_at'),
    ('reviews', 'title', 'company_name', 'created_at');

-- Count most common phrases in the reviews table for 1 star reviews
select unnest(pg_phrase.extract(body)) as phrase, count(*) 
from reviews
where rating = 1
group by phrase
order by count desc
limit 100;
```
