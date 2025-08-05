# TLA+ Training

## TLA + Online Training (***learntla***)


### Conceptual Overview

- The purpose of TLA+, then, is to programmatically explore design issues.
- There are three parts of the conceptual framework of TLA+
  - Specification
    - The specification has a set of "behaviors", or possible distinct executions. For the spec to be correct, every behavior must satisfy all of our system requirements, or properties. "No account can overdraft" is an example property, and is violated if there's a behavior of the spec has a state where an account has a negative balance.
		> "No account can overdraft" is an **invariant** property, which is one that must be true of every single state of every single behavior. There are other, more advanced properties, like **liveness** and **action** properties
	- Once we've written a spec and properties, we feed them into a "model checker". The model checker takes the spec, generates every possible behavior, and sees if they all satisfy all of our properties


- ***Ex- Wire Transfer***
> In this example race condition if we intitate two transfer at time, such issue can be detected by TLA+
```python
def transfer(from, to, amount)
	if(amount <= from.balance)
		from.balance -= amount;
		to.balance +=amount;
```
