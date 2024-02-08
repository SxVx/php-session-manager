# :elephant: PHP SESSION MANAGER :bookmark_tabs:

## What is?

It's an example of a solid principle focused primarily on dependency injection.

In this example there are three ways to manage the session, default php files (default php.ini configuration), management with custom files, and management with redis.

## Problematic
The problematic is when you must have work with different servers or environments, or you want to do a PoC (Prof of Concept) and you don't know if the server is available or not for a specific configuration, so you define a class with a special configuration and this configuration can will be extended or have variation

The problem is when you have to work with different servers or environments, or you want to do a PoC (Proof of Concept) and you don't know if the server is available or not for a specific configuration, then you define a class with a special configuration and this configuration can be expand or have a variation