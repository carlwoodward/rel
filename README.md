# Rel

https://github.com/yang/rel

## Description

Rel is a SQL AST manager for Node JS. It is a straight port of https://github.com/rails/arel. Although it does have some changes of note. These are:

1. No reliance on a database connection. This library builds queries
   only.
2. It obviously can't use all of the ruby-isms like over-riding the
   array operator so methods are used instead.

It still holds the same goals as Arel which are:

1. Simplifies the generation complex of SQL queries.
2. Adapts to various RDBMS systems

_Before you ask, there will also be a port of ActiveRecord coming in the next little bit._

## Installation

    npm install rel

Unfortunately the package is currently purely Coffeescript, with no
accompanying JS compilation in the git repo.  To use this with a JS project,
you can compile the files in-place with:

    npm run build

This is a bandaid for the time being.

## Introduction

    users = new Rel.Table 'users'
    users.project(Rel.star()).toSql()

Will produce

    SELECT * FROM users

A more complicated example of command queries is:

    users.where(users.column('name').eq('amy'))
    # => SELECT * FROM users WHERE users.name = 'amy'

In SQL the selection would contain what you are getting from the
database, this is called a projection in Rel.

    users.project(users.column('id')) # => SELECT users.id FROM users

Joins resemble SQL:

    users.join(photos).on(users.column('id').eq(photos.column('user_id')))
    # => SELECT * FROM users INNER JOIN photos ON users.id = photos.user_id

Limit and offset and called __take__ and __skip__:

    users.take(5) # => SELECT * FROM users LIMIT 5
    users.skip(4) # => SELECT * FROM users OFFSET 4

GROUP BY is called group:

    users.group(users.column('name')) # => SELECT * FROM users GROUP BY name

You can chain all operators, for example:

    users.where(users.column('name').eq('amy')).project(users.column('id'))
    # => SELECT users.id FROM users WHERE users.name = 'amy'

Another example:

    users.where(users.column('name').eq('bob')).where(users.column('age').lt(25))

You can also pass in multiple arguments:

    users.where(users.column('name').eq('bob'), users.column('age').lt(25))

The OR operator works like this:

    users.where(users.column('name').eq('bob').or(users.column('age').lt(25)))

This is the same as the AS operator.

## Contributors

Founded and majority of development to date by Carl Woodward ([@carlwoodward]).

Current developer and maintainer is Yang Zhang ([@yang]).

## Development

To run tests, run `npm test`.

## Setup notes

- Using special commit of vows to work around
  <https://github.com/flatiron/vows/issues/296>.

[@carlwoodward]: //github.com/carlwoodward
[@yang]: //github.com/yang
