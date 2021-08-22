## Google Python Style Guide

https://google.github.io/styleguide/pyguide.html

Python Style Guide Rules !!

- <span style="color:red">`가독성`</span>
- <span style="color:red">`일관성`</span>

```python
No:
    
def add_two_numbers(number1, number2):
    return number1 + number2

```

```python
Yes: 
    
def add_two_numbers(number1, number2):
    """Returns the sum of two numbers
    Args:
        number1 (int): First number to add
        number2 (int): Second number to add
    Returns:
        int: Sum of `number1` and `number2`
   """
   return number1 + number2
```

Examples : 

```python
No:
    
def discount_rewards(r, gamma=FLAGS.gamma):
    discounted_r = np.zeros_like(r)
    running_add = 0
    for t in reversed(range(0, r.size)):
        if r[t] != 0:
            running_add = 0
        running_add = running_add * gamma + r[t]
        discounted_r[t] = running_add
return discounted_r
```

```python
Yes:

def discount_rewards(rewards, gamma=FLAGS.gamma):
    """Returns discounted rewards by a rate, `gamma`
    
    When a reward is nonzero, 
    the game has been reset ("Pong" specific)
    
    Args:
        rewards (1-D Array): Rewards of shape (n_samples,)
        gamma (float, optional): Discount rate
    
    Returns:
        1-D Array: Discounted rewards of shape (n_samples,)
    
    Example:
        >>> r = np.array([1, 1, 1])
        >>> discount_rewards(r, 0.99)
        np.array([1 + 0.99 + 0.99**2, 1 + 0.99, 1])
    """
    discounted_r = np.zeros_like(rewards)
    return discounted_r
```

