# Backend Tutorial with Golang

## Table of Contents

- [Project Description](#project-description)
- [Application Description](#application-description)
- [Database Schema Design](#database-schema-design)

## Project Description

In this project, I will follow a [tutorial series](https://youtu.be/rx6CPDK_5mU), learn how to create a complete backend system for **simple banking application**. There are **three** parts in this project including **design**, **develope** and **deploy**. I will design database schema for PostgreSQL, develope API with Golang and deploy the application with Docker.

## Application Description

I will create a simple banking application including these functions:
- create and manage account
- record any changes of accounts
- support transfering money between accounts

## Database Schema Design

I will use [dbdiagram.io](https://dbdiagram.io/home) to design the database schema, and export schema as PostgreSQL code. If you are not familiar with this tool, please take 10 minutes to read the [docs](https://www.dbml.org/docs/#syntax-consistency).

I design three tables according to the three banking functions listed above.

- accounts table: each record represents an account
- entries table: each record represents a change of an account
- transfers table: each record represents a transfer action between two accounts

![database schema](https://github.com/johnnyhwu/Backend-Tutorial-with-Golang/blob/main/assets/images/database%20schema.png)
