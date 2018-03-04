# NNPush Python Library Integration Examples

Find more information about project at [deepdescent.net](deepdescent.net).

This demonstrates using the NNPush python library to monitor and track
your neural network tracking. This library is designed to be used in
conjunction with the mobile iOS app [here](https://itunes.apple.com/us/app/nnpush/id1324430703?ls=1&mt=8).

## Installation

* install library using pip: `pip install nnpush`
* nnpush requires using python 3.x at the moment.

## Pairing New Project to Device

Call `qr_connect()` to generate QR to scan from app to pair.
```
def qr_connect():
	"""Create project and display QR code to scan from app to connect.
	
	Project will only be created one first call, subsequent `qr_connect`
	calls will return same project.

	Args:
		Saves QR code image to file and displays it in output if within
		a jupyer notebook. Project token and id are printed out as well.

	"""
```
Example:
```
	import nnpusher as npush

	npush.qr_connect()
```
Jupyer notebook output will display QR to scan from app.

An image of the QR code is also saved in local directory.

Click red plus button to open camera scanner from within app.


## Run Using Existing Project

To connect an existing project, you need that project's token
and project id. Use the function `npush.token_connect(token:pid:)`.
```
def token_connect(token, pid):
	"""Connect library to existing project.
	
	Connect library to existing project using token and pid.

	Args:
		token: project token in format 00000000-0000-0000-0000-000000000000
		pid: project id, e.g. 5

	"""
```
Example:

```
	import nnpusher as npush

	npush.token_connect(token='01e2ea10-16da-11e8-89ff-53dab96c793e', pid='95')
```

## Specify Hyperparameters to Track

Before running training if you want your iterations to keep track of
hyperpameter values and their changes between iterations, use `track_hyperparameters`
to capture for global variable hyperparameters.
```
def track_hyperparameters(params):
	"""Keep track of live hyperparameters.
	
	Add array of variables to be tracked between iterations. If iterations
	are changed between iterations, the updated values are reflected.

	The hyperparameter variables must exist at the global level in the
	main module.

	Args:
		params: array of hyperparameter variables

	"""
```
Example:
```
	#NOTE: params all must be on __main__ module in global scope
	npush.track_hyperparameters([
	    batch_size,
	    z_dim,
	    learning_rate,
	    beta1])
```

## Create, Update, Complete Iterations
An iteration represents a full training cycle for a specific set of hyperparameters.
Create an iterations with `start_iteration()`.
```
def start_iteration():
	"""Create new iteration to send loss data to.

	On creation, current hyperparameters, if they are being tracked by
	`track_hyperparameters` function, will retrieve current variable
	values to be associated with this iteration at this point.

	Returns:
		returns session id for created iteration.
  
	"""
```

Update an iterations with `update_iteration(sid:loss:)`
```
def update_iteration(sid, loss):
	"""Updates an iteration with loss data point.

	Loss is sent to be associated with an iteration represented by
	session id.

	This call does add some extra overhead to training and so should
	be used sparingly and probably not on every single data point
	otherwise training time may become unnecessarily increased.

	Args:
		sid: session id of iteration to update
		loss: loss data point to associate with iteration

	"""
```

Complete an iteration with `complete_training(sid:)`
```
def complete_training(sid):
	"""Mark an iteration as completed.

	This will mark the iteration as having completed.

	This will also trigger a push notification notifying the paired
	device that the iteration is compelted.

	Args:
		sid: session id of iteration to update

	"""
```

Example:
```
    #start a training iteration
    session_id = npush.start_iteration()

    #update with loss 10 times
    for i in range(0,10):
        loss = random.random()
        npush.update_iteration(session_id, loss)
    
    #complete training iteration
    npush.complete_training(session_id)
```

## Complete Project
Mark your project as complete using `complete_project()` call.
```
def complete_project():
	"""Mark a project as completed.

	This will mark the project as having completed.

	This will also trigger a push notification notifying the paired
	device that the project is compelted.

	"""
```

Example:
```
	npush.complete_project()
```

