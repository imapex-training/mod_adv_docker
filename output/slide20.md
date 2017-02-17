
* Build a new version of the container
	* Note how the process takes much less time and the references to "Using cache".  Because there has been no change in the earlier steps, docker needn't rerun them
	* This is why proper consideration in the order of steps in your container creation is critical.  
	* Keep the steps likely to change (ie Copying in your code) to the end.  

