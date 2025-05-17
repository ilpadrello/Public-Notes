We all know that JS is not the best in Error handling, we got different problems when it comes to handling errors. There are "involuntary errors" and "known errors".

The "involuntary errors" are errors that JS throw, while the "know errors" could be business logic errors.

JS make everything more difficult when it comes to Async code.
What you do ? you put a .catch for each async call you do ?

# SOLUTION THAT I HAVE FOUND
The solution I have found is to wrap all the Async calls into a tryCatch `tc` to solve the problem, here's un example:

