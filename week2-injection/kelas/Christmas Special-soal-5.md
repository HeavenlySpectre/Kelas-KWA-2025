# Christmas Special - OWASP JUICE SHOP

## Challenge Description

* **Category**: Injection
* **Difficulty**: ⭐⭐⭐⭐
* **Description**: Order the Christmas special offer of 2014.
* **Link Resource**: `http://localhost:3000/#/score-board?categories=Injection`

## Solution

### Step 1: Find the products

* We can exfiltrated all the products and details through database using `'))%20--%20` payload and directly find the product

![findchristmas](../../assets/christmas%20special1.png)

* We also know the complete information of the product

```
      "id": 10,
      "name": "Christmas Super-Surprise-Box (2014 Edition)",
      "description": "Contains a random selection of 10 bottles (each 500ml) of our tastiest juices and an extra fan shirt for an unbeatable price! (Seasonal special offer! Limited availability!)",
      "price": 29.99,
      "deluxePrice": 29.99,
      "image": "undefined.jpg",
      "createdAt": "2025-09-09 02:15:51.284 +00:00",
      "updatedAt": "2025-09-09 02:15:51.284 +00:00",
      "deletedAt": "2025-09-09 02:15:51.426 +00:00"
```

### Step 2: Add to basket

* We will intercept the request that sent if we add some products to the basket.
* It send a POST api request to server with information like ProductId, BasketId, and quantity
* We'll change the ProductId to `10` since the `Christmas Super-Surprise-Box (2014 Edition)` is using the Id.
* We'll send the request with our given ProductId

![addingbasket](../../assets/christmas%20special2.png)


### Step 3 : Basket and Checkout

* For the response, we got the success status and we find out that we successfully added the product to our basket.
* Checkout to finish the challenge.

## Result

We are able to order the Christmas special offer of 2014.

![Result](../../assets/christmas%20special3.png)

## Explanation

The vulnerability exploited in this challenge is a **Union-based SQL Injection** on the product search functionality, which allowed us to enumerate hidden or deleted products from the database. By injecting crafted payloads, we were able to exfiltrate details of the `Christmas Super-Surprise-Box (2014 Edition)` product, including its internal `id`. The lack of proper input sanitization enabled direct access to sensitive product metadata. The attack demonstrates how SQL Injection combined with insecure API design can lead to unauthorized actions such as ordering discontinued items.
