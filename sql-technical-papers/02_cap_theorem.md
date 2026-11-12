# Technical Paper 02: CAP Theorem in Distributed Databases

**Subject:** Database Management Systems  
**Topic:** CAP Theorem  
**Focus:** Distributed Systems and Data Availability

---

## 1. Introduction to the CAP Theorem

The CAP Theorem explains an important trade-off in distributed systems.

A distributed system stores or processes data across multiple computers, often called **nodes**. These nodes communicate over a network and work together as one system.

CAP stands for:

1. **Consistency**
2. **Availability**
3. **Partition Tolerance**

The theorem states that when a network partition occurs, a distributed system must choose between consistency and availability.

This does not mean that a system can only ever support two of the three properties. The trade-off matters when a network partition prevents some nodes from communicating.

## 2. Consistency

**Definition:** In the CAP Theorem, consistency means that every successful read receives the latest completed write, or an error.

All nodes behave as if they share one up-to-date copy of the data.

### Example

Suppose an online wallet stores a balance of ₹5,000.

A user adds ₹2,000, and the write is completed. The new balance is ₹7,000.

If the system is consistent, a read from any node that successfully responds must return ₹7,000, not the old value of ₹5,000.

**Important:** CAP consistency is not the same as the Consistency property in ACID. ACID consistency is about keeping database rules and constraints valid. CAP consistency is about seeing the latest data across distributed nodes.

## 3. Availability

**Definition:** Availability means that every request sent to a working node receives a response, even if that response may contain older data.

The system does not refuse a request simply because another node cannot be reached.

### Example

Suppose a shopping website has servers in Mumbai and Delhi.

If the network connection between the two locations fails, the Mumbai server may continue responding to customers in Mumbai.

It may show slightly older product stock information, but it remains available.

## 4. Partition Tolerance

**Definition:** Partition tolerance means that the system continues operating even when a network failure prevents some nodes from communicating with each other.

A network partition can happen because of:

- Network cable or router failure
- Data centre connectivity problems
- Network delays or packet loss
- Firewall or configuration errors

### Example

Imagine a database with two nodes:

- Node A is in Mumbai.
- Node B is in Delhi.

If the network connection between them fails, they cannot exchange updates.

The system must decide how to handle requests while the connection is unavailable.

## 5. Understanding the CAP Trade-off

When a network partition occurs, a distributed system generally chooses one of two approaches.

### CP: Consistency and Partition Tolerance

A CP system protects consistency during a partition. It may reject or delay some requests until it can safely confirm the latest data.

**Example:** A bank balance system may refuse a withdrawal when it cannot confirm the latest balance. This helps prevent spending the same money twice.

Advantages:

- Protects the latest agreed data.
- Useful when incorrect or conflicting updates are unacceptable.

Disadvantage:

- Some requests may fail or wait during a partition.

### AP: Availability and Partition Tolerance

An AP system continues responding during a partition, even if some responses contain older data.

Different nodes may temporarily accept updates. The system must resolve or merge those updates after communication returns.

**Example:** A social media application may allow users to like a post even when some servers cannot communicate. The like count may take time to become the same everywhere.

Advantages:

- More requests can receive a response during a partition.
- Useful when temporary differences are acceptable.

Disadvantage:

- Users may temporarily see stale or conflicting data.

### CP and AP Comparison

| Feature | CP Approach | AP Approach |
|---|---|---|
| Main priority during a partition | Consistency | Availability |
| Requests during a partition | May be rejected or delayed | Usually receive a response |
| Data freshness | Protects the latest agreed data | May return older data |
| Common use | Banking and critical records | Social feeds and some shopping features |

## 6. What About CA?

CA means Consistency and Availability.

A system can provide consistency and availability when there is no network partition. However, a distributed system cannot guarantee both during a partition while also tolerating that partition.

A single-node database may provide consistency and availability under normal conditions, but it does not provide distributed partition tolerance in the same sense.

## 7. Real-World Examples

| Application | Possible Design Choice | Reason |
|---|---|---|
| Bank balance updates | CP-style behaviour | Avoid incorrect balances |
| Social media feeds | AP-style behaviour | Keep feeds available |
| Product recommendations | AP-style behaviour | Temporary stale data is acceptable |
| Inventory checkout | Often consistency-focused | Avoid selling unavailable stock |

These are simplified examples. Real applications may use different choices for different features.


## 8. Key Takeaways

- CAP stands for Consistency, Availability, and Partition Tolerance.
- The trade-off between consistency and availability matters during a network partition.
- CP systems may reject or delay requests to protect consistency.
- AP systems continue responding but may return stale data.
- The right design depends on what the application can safely tolerate.

## 9. Conclusion

The CAP Theorem explains the challenges of maintaining consistency and availability in distributed database systems when network failures occur. It helps developers understand the trade-offs involved when designing distributed applications.

Choosing between consistency and availability depends on the application's requirements. Understanding the CAP Theorem helps in selecting suitable database architectures for systems that need to handle network failures and multiple nodes.
