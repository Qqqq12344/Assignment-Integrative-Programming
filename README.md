## Order & Payout Module

The Order and Payout Module manages the full purchasing workflow within the Shobee platform. It is responsible for creating orders, managing order items, tracking order status, confirming payments, and preparing vendor payouts.

This module ensures that customer purchases are processed securely while allowing vendors to manage their sales efficiently. It also integrates with Stripe to support secure online payments and automated payment verification.

## Order Management

The system records customer purchases through an Order entity, which stores information such as the customer ID, vendor ID, total price, and payment reference.

Each order contains multiple Order Items, representing the products purchased, including quantity, price, and selected product variations. This structure allows the platform to maintain clear relationships between customers, vendors, and products.

## State Pattern

The order workflow is implemented using the State Design Pattern. Instead of relying on conditional logic to control order status transitions, each order state is represented by a separate state class.

Typical order states include: Draft , Paid , Shipped , Delivered , Cancelled , Failed

Each state defines the allowed actions, such as progressing to the next stage or cancelling the order. This improves maintainability and prevents invalid state transitions.

## Stripe Payment Integration

The system integrates with Stripe Checkout and Stripe Webhooks to handle secure payment processing.

When a payment is completed, Stripe sends webhook events to the application. These events trigger backend operations such as:

-Updating the order status to Paid

-Calculating platform and payment processing fees

-Recording vendor earnings

-Updating product inventory

-Sending order confirmation notifications

-Using webhooks ensures that payment verification is handled securely on the server side.

## Vendor Commission & Payout

After payment confirmation, the system calculates how the transaction amount is distributed:

-Payment processing fee (Stripe fee)

-Platform commission

-Vendor earnings

These calculations allow the platform to prepare accurate payout records for vendors. The system also supports Stripe Connect onboarding, enabling vendors to receive payouts through connected Stripe accounts.

## REST API Integration

The Order module exposes RESTful APIs to allow communication with other system modules.

Examples include:

The Cart Module communicating with the Order Module when checkout is completed.

The Vendor Module retrieving orders associated with a specific vendor.

The Product Module updating inventory after successful purchases.

This modular architecture ensures the system remains scalable and loosely coupled.
