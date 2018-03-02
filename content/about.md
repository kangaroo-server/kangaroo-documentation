---
title: About the Project
menu:
    main:
        weight: 200
---

## Contributing

We maintain an active list of [issues](https://github.com/kangaroo-server/kangaroo/issues), and use 
github's [project tracker](https://github.com/kangaroo-server/kangaroo/projects)
for larger bodies of work. To coordinate and ensure that your work is not 
being duplicated, please reach out the the community in the [IRC Channel](http://webchat.freenode.net/?channels=kangaroo).

## Philosophy

The project adheres to several philosophies that impact our design decisions and architecture.

### Secure by Default

In order to encourage a safe and secure internet, the project will default to the most
secure configuration. This is to address both overtaxed engineers and operators, as many
do not have the time to read all the documentation, or test all the configurations.

### Test Everything

We expect 100% test coverage. While we understand that this does not 
necessarily guarantee good tests, it does force a contributing engineer to 
think through edge cases in a way that might otherwise be skipped. It also
puts us in a mindset to write testable code, and thus has a substantial impact
on our project's internal plumbing.

### Choose Boring Technology

We adhere to the doctrine of [Choose Boring Technology](http://mcfunley.com/choose-boring-technology). 
This is done mostly out of a need for project and engineer sanity sanity; if development, 
packaging, and deployment get too complex, we risk several things:
 
 * Learning Curve: Every new tool requires new and potentially unrelated knowledge, hindering new contributors.
 * Deployment complexity: Running the system in production becomes prohibitively complex.
 * Experts only: The public image of the project itself becomes intimidating.
 * Disconnected Scaling: Separate components scale at different rates.
