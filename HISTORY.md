# Command History

The command history is used for restoring the project to previous states.

A Command needs to be able to serialized which means it inherits from the 
Object class and impliment Object::get/set_properties.

If Commands can contain references to functions or methods for restoring state
then those functions need to be registered in the TypeSystem...Or the Command
classes need to be registered?

A Command can contain either a FunctionObject or a MethodObject? 

The properties of a command are named arguments.

When a Command is executed the named arguments are used when calling the 
underlying function.

A Command is an interface
 
A Transaction:
 - contains a series of Commands.
 - inherits from Object and is serializable.

A History:
 - contains a series of Transations.
 - inherits from Object and is serializable.
