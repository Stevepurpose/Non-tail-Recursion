# Non-tail Recursion
Non-tail recursion as we know is a type of recursion where the recursive call is not the last line of code executed in a recursive call. Occassionally we experience a type of this recursion that returns a value at base case. This value is then returned to lower layers of the stack for certain operations to be performed before they are popped off the stack.
Below we want to calculate the sum of the first 6 natural numbers using recursion.

```javascript
function sum(n){
    if(n==1) return 1;
    let s = n + sum(n-1)
    return s
}
let a = sum(6)
console.log(a)
```

## The Breakdown
At the beginning of this program control moves to the line  `let a = sum(6)` . This adds `sum(6)` to the stack
after which the base case is checked. we need a `true` for our recursive calls to terminate. Currently it is a `false`, so the function continues executing. At the point `sum(n-1)`, the function is called again. This time will be `sum(6-1)` , that is `sum(5)`. Now we have a pending operation on hold  which is the addition of the result of our `sum(5)` with `n=6` . so we have a pending `6 + sum(5)` in our `sum(6)`  call still on the stack. So `sum(5)` starts running , it checks base case and moves to the next line. At the point `sum(n-1)` , the function is called again which now is  `sum(5-1)` which is `sum(4)`. Now we have a pending `5 + sum(4)` in our `sum(5)` call. Now this whole process continues and then we have on our stack:

```javascript
2 + sum(1)
3 + sum(2)
4 + sum(3)
5 + sum(4)
6 + sum(5)
```
Now for the last call `sum(1)`, the base case is true so it returns `1` to the point it was called from . see below:

```javascript
n==1 return 1
2 + sum(1)
3 + sum(2)
4 + sum(3)
5 + sum(4)
6 + sum(5)
```
So it pops off the stack returns to `sum(1)` on the line `2 + sum(1)`.  Which will be `2+1` that is `3`, this stack frame is popped off and `3` is returned to the next line where it is called from.  This point is `sum(2)` on the line `3 + sum(2)` so we have `3 + 3` which is `6`. This frane is then popped off the stack and `6` is returned to the point `sum(3)` on the line `4 +sum(3)` which is `4+6` that is `10`. This line is popped off and `10` is returned to the line `5 + sum(4)` which is `5 + 10` that is `15`. `15` is returned to the line `6 + sum(5)` which will be `6+15` that is `21`.. when we run our code we get exactly that.

So we can see how certain returned values are used for pending operations in our recursive calls.