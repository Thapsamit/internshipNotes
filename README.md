## Difference between forEach vs for of loop in asyn/await 


### forEach Loop:
The forEach loop doesn't inherently wait for asynchronous operations (like functions using async/await) to complete before moving on to the next iteration. When an asynchronous operation is encountered within the loop, it is started, but the loop doesn't pause to wait for it to finish. As a result, the loop might iterate through all elements of the array before any of the asynchronous operations have completed.

In your original code with the forEach loop, multiple LiveClassRoomFile.create operations were being initiated in parallel, but the loop didn't wait for them to complete before moving to the next iteration. This could lead to the filesResArray being empty or incomplete when you used it later.

### for...of Loop:
The for...of loop, on the other hand, is synchronous and works well with async/await. In each iteration, the loop will pause and wait for the asynchronous operation to complete before moving to the next iteration. This ensures that each asynchronous operation is completed before the loop moves on to the next element.

By using the for...of loop as shown in the corrected code, you're ensuring that each LiveClassRoomFile.create operation completes before you proceed to the next iteration, and the results are added to the filesResArray as expected.
