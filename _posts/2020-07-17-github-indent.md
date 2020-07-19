---
date: Fri Jul 17 2020 11:01:18 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Auto-indenting code blocks in GitHub comments
previous_teaser: New names for kinds of lava based on how they feel
previous_first: true
---
When you're writing a code block in GitHub's comment entry box, under what circumstances will it indent a new line?

| language         | closing backticks present | code block created by        | intenting with  | content     | where                  | result                                       |
|------------------|---------------------------|------------------------------|-----------------|-------------|------------------------|----------------------------------------------|
| **`suggestion`** | **yes**                   | "Insert a suggestion" button | tabs            | statement   | PR line comment        | **auto indent**                              |
| **`suggestion`** | **yes**                   | "Insert a suggestion" button | spaces          | statement   | PR line comment        | **auto indent**                              |
| **`suggestion`** | **yes**                   | manual entry                 | tabs            | statement   | PR line comment        | **auto indent**                              |
| **`suggestion`** | **yes**                   | manual entry                 | spaces          | statement   | PR line comment        | **auto indent**                              |
| `suggestio`      | yes                       | manual entry                 | tabs            | statement   | PR line comment        | no auto indent                               |
| `suggestio`      | yes                       | manual entry                 | spaces          | statement   | PR line comment        | no auto indent                               |
| `js`             | yes                       | manual entry                 | tabs            | statement   | PR line comment        | no auto indent                               |
| `js`             | yes                       | manual entry                 | spaces          | statement   | PR line comment        | no auto indent                               |
| (absent)         | yes                       | manual entry                 | tabs            | statement   | PR line comment        | no auto indent                               |
| (absent)         | yes                       | manual entry                 | spaces          | statement   | PR line comment        | no auto indent                               |
| `suggestion`     | no                        | manual entry                 | spaces          | statement   | PR line comment        | no auto indent                               |
| **`suggestion`** | **yes**                   | manual entry                 | tabs and spaces | statement   | PR line comment        | **auto indent**, same sequence of whitespace |
| **`suggestion`** | **yes**                   | manual entry                 | spaces and tabs | statement   | PR line comment        | **auto indent**, same sequence of whitespace |
| **`suggestion`** | **yes**                   | manual entry                 | tabs            | block open  | PR line comment        | **auto indent**, same level                  |
| **`suggestion`** | **yes**                   | manual entry                 | tabs            | block close | PR line comment        | **auto indent**, same level                  |
| **`suggestion`** | **yes**                   | manual entry                 | tabs            | blank       | PR line comment        | **auto indent**, same level                  |
| **`suggestion`** | **yes**                   | manual entry                 | spaces          | statement   | PR comment             | **auto indent**                              |
| **`suggestion`** | **yes**                   | manual entry                 | spaces          | statement   | new PR description     | **auto indent**                              |
| **`suggestion`** | **yes**                   | manual entry                 | spaces          | statement   | edit PR description    | **auto indent**                              |
| **`suggestion`** | **yes**                   | manual entry                 | spaces          | statement   | review changes comment | **auto indent**                              |
| **`suggestion`** | **yes**                   | manual entry                 | spaces          | statement   | new issue              | **auto indent**                              |
| `suggestion`     | yes                       | manual entry                 | spaces          | statement   | commit changes description | no auto indent                               |
| (absent)         | yes                       | manual entry                 | spaces          | statement   | file editor            | auto indent                                  |
| (absent)         | no                        | manual entry                 | spaces          | statement   | file editor            | auto indent                                  |

What seems to be relevant:

- Opening backticks _and_ closing backticks present.
- Language is set to `suggestion`.
