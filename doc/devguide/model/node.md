## Node Model

### Aliveness and availability:

Nodes in the Crowbar framework have two related flags that control
whether the annealer can operate on them.

Aliveness is under the control of the Crowbar framework and
encapsulates the framework's idea of whether any given node is
manageable or not.  If a node is pingable and can be SSH'ed into as
root without a password using the credentials of the root user on
the admin node, then the node is alive, otherwise it is dead.
Aliveness is tested everytime a jig tries to do something on a node
-- if a node cannot be pinged and SSH'ed into from at least one of
its addresses on the admin network, it will be marked as
dead.  When a node is marked as dead, all of the noderoles on that
node will be set to either blocked or todo (depending on the state of
their parent noderoles), and those changes will ripple down the
noderole dependency graph to any child noderoles.

Nodes will also mark themselves as alive and dead in the course of
their startup and shutdown routines.

Availability is under the control of the Crowbar cluster
administrators, and should be used by them to tell Crowbar that it
should stop managing noderoles on the node.  When a node is not
available, the annealer will not try to perform any jig runs on a
node, but it will leave the state of the noderoles alone.

A node must be both alive and available for the annealer to perform
operations on it.
