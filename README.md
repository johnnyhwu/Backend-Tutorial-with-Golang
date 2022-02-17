# Backend Tutorial with Golang

## Table of Contents

- [Project Description](#project-description)
- [Application Description](#application-description)
- [Database Schema Design](#database-schema-design)
- [Setup Docker and PostgreSQL](#setup-docker-and-postgresql)

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

Below are complete database schema and code: 

![database schema](https://github.com/johnnyhwu/Backend-Tutorial-with-Golang/blob/main/assets/images/database%20schema.png)

```
Table accounts as A {
  id bigserial [pk]
  owner varchar [not null]
  balance bigint [not null]
  currency varchar [not null]
  created_at timestamptz [not null, default: `now()`]

  Indexes {
    owner
  }
}

Table entries {
  id bigserial [pk]
  account_id bigint [ref: > A.id]
  amount bigint [not null]
  created_at timestamptz [not null, default: `now()`]

  Indexes {
    account_id
  }
}

Table transfers {
  id bigserial [pk]
  from_account_id bigint [ref: > A.id]
  to_account_id bigint [ref: > A.id]
  amount bigint [not null]
  created_at timestamptz [not null, default: `now()`]

  Indexes {
    from_account_id
    to_account_id
    (from_account_id, to_account_id)
  }
}
```

It is easy to generate PostgreSQL code from [dbdiagram.io](https://dbdiagram.io/home).

```
CREATE TABLE "accounts" (
  "id" bigserial PRIMARY KEY,
  "owner" varchar NOT NULL,
  "balance" bigint NOT NULL,
  "currency" varchar NOT NULL,
  "created_at" timestamptz NOT NULL DEFAULT (now())
);

CREATE TABLE "entries" (
  "id" bigserial PRIMARY KEY,
  "account_id" bigint,
  "amount" bigint NOT NULL,
  "created_at" timestamptz NOT NULL DEFAULT (now())
);

CREATE TABLE "transfers" (
  "id" bigserial PRIMARY KEY,
  "from_account_id" bigint,
  "to_account_id" bigint,
  "amount" bigint NOT NULL,
  "created_at" timestamptz NOT NULL DEFAULT (now())
);

ALTER TABLE "entries" ADD FOREIGN KEY ("account_id") REFERENCES "accounts" ("id");

ALTER TABLE "transfers" ADD FOREIGN KEY ("from_account_id") REFERENCES "accounts" ("id");

ALTER TABLE "transfers" ADD FOREIGN KEY ("to_account_id") REFERENCES "accounts" ("id");

CREATE INDEX ON "accounts" ("owner");

CREATE INDEX ON "entries" ("account_id");

CREATE INDEX ON "transfers" ("from_account_id");

CREATE INDEX ON "transfers" ("to_account_id");

CREATE INDEX ON "transfers" ("from_account_id", "to_account_id");
```

## Setup Docker and PostgreSQL

In this step, I will install Docker and run PostgreSQL container. Moreover, I will use Table Plus to interact with PostgreSQL to create database schema.

- 01 : Install Docker

  Just download and install Docker from its [official website]().

