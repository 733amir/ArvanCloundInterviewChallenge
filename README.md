# Services:

* Wallet
    * Balance
    * Transaction
* Discount
    * Percentage
    * Free Credit 

# Senario

It's the final match between Iran and Brazil. We have discount code (free credit
type). It will add ten million Rials to first 1000 people using it.

# Requirements

* Online report of people getting discount.
* Online balance.
* No auth (request is (mobile, code))
* No UI, only API

# REST Endpoints

| Method |     Endpoint         |            Example Data            |
|:-------|:---------------------|:-----------------------------------|
| PUT    | /transactions        | {"mobile": "", "discountCode": ""} |
| GET    | /discounts/:id       | Discount info TODO                 |
| GET    | /discounts/:id/stats |                                    |
| GET    | /wallets/:mobile     | Wallet Info TODO                   |

# Architecture

## Wallet

### PostgreSQL

    transactions (create, read)
        id uuid primary key
        wallet_id uuid foreign key
        discount_id uuid foreign key?
        create_time timestamptz
        type text (charge, iaas_invoice, cdn_invoice, ...)
        amount double

    wallet (create, read, update)
        id uuid primary key
        create_time timestamptz
        mobile text unique
        balance double

## Discount

### PostgreSQL

    discount (create, read, update)
        id uuid primary key
        create_time timestamptz
        enable_time timestamptz
        type text
        code text unique
        value double

    applied_discount
        discount_id uuid primary key
        mobile uuid primary key
        create_time timestamptz
