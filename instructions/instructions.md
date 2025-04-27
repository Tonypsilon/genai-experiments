# Coding Instructions and Style Guide

## Table of Contents
- [Domain-Driven Design (DDD)](#domain-driven-design-ddd)
- [Clean Code Principles](#clean-code-principles)
- [Modern Java Practices](#modern-java-practices)
- [Code Structure](#code-structure)
- [Testing Guidelines](#testing-guidelines)

## Domain-Driven Design (DDD)

### Ubiquitous Language
- Use consistent terminology between domain experts and developers
- Define a glossary of domain terms for the project
- Reflect domain language in class names, method names, and variables
- Example: If domain experts talk about "Policies", don't use "Contracts" in code

### Bounded Contexts
- Identify explicit boundaries between different parts of the domain
- Each bounded context has its own ubiquitous language
- Define context maps to show relationships between bounded contexts
- Separate models in different packages/modules by bounded context

### Entities vs Value Objects
- Use Value Objects wherever possible (immutable, equality by attributes)
- Entities have identity, Value Objects do not (focus on their attributes)
- Make Value Objects always valid and immutable
- Example: `Money(amount, currency)` as a Value Object rather than primitives

### Aggregates
- Design clear aggregate boundaries with a single root entity
- Keep aggregates small and focused on a specific business capability
- Reference other aggregates by ID, not direct object references
- Ensure transactional consistency only within an aggregate

### Domain Events
- Use domain events to communicate between bounded contexts
- Model significant state changes as explicit events
- Name events in past tense (e.g., `OrderPlaced`, `PaymentReceived`)
- Include all necessary context in the event payload

## Clean Code Principles

### Tell, Don't Ask
- Objects should tell other objects what to do, not ask about their state
- Encapsulate behavior with data it manipulates
- Avoid sequences of getters followed by decisions
- Example: `customer.purchase(product)` instead of `if (customer.getBalance() > product.getPrice()) {...}`

### Single Responsibility Principle
- Each class should have only one reason to change
- Extract separate concerns into their own classes
- Functions should do one thing and do it well
- Keep functions under 20 lines if possible

### Law of Demeter
- Talk only to immediate friends, not strangers
- Avoid method chains like `a.getB().getC().doSomething()`
- Example: Use `customer.getAddress()` rather than `customer.getDetails().getAddress()`

### Meaningful Names
- Choose descriptive names that reveal intent
- Use domain terminology from ubiquitous language
- Method names should indicate their behavior and side effects
- Class names should be nouns, method names should be verbs

### Comments
- Code should be self-explanatory without comments when possible
- Use comments to explain "why" not "what" or "how"
- Keep comments up-to-date with code changes
- Consider refactoring unclear code instead of adding comments

## Modern Java Practices

### Immutability
- Make classes immutable by default
- Use constructor injection for required dependencies
- Make fields final whenever possible
- Return defensive copies of mutable objects

### Records for Value Objects
- Use Java records for immutable Value Objects
- Example: `record Money(BigDecimal amount, Currency currency) {}`
- Add validation in compact constructors
- Extend records with methods that express domain behavior

### Functional Programming
- Use streams for collection operations
- Prefer pure functions without side effects
- Use Optional to handle nullable return values properly
- Leverage method references when appropriate: `orders.stream().map(Order::getTotal)`

### Visibility Control
- Make everything as restricted as possible by default
- Use private fields with controlled access
- Package-private (default) for classes used only within a package
- Protected only when necessary for inheritance

### Effective Enums
- Use enums for fixed sets of related constants
- Add behavior to enums through methods
- Use EnumMap/EnumSet for collections of enum values
- Consider factory methods for complex enum construction

### Type Safety
- Leverage the type system to prevent errors
- Create specific types rather than using primitives
- Example: `CustomerId` instead of `Long` for IDs
- Use sealed classes/interfaces to restrict inheritance hierarchies

## Code Structure

### Package by Feature
- Organize code by business feature, not technical layer
- Group related classes together regardless of their type
- Example: `com.example.ordering` instead of `com.example.controllers`
- Enables better encapsulation of feature implementations

### Dependency Injection
- Use constructor injection for required dependencies
- Field injection only for optional dependencies
- Make dependencies explicit in the class API
- Configure a DI container (Spring) for wiring

### Exception Handling
- Use checked exceptions only for recoverable conditions
- Create domain-specific exception types
- Include relevant context in exception messages
- Handle exceptions at appropriate levels of abstraction

### Interface Design
- Design interfaces based on client needs
- Keep interfaces small and focused (Interface Segregation)
- Prefer composition over inheritance
- Use default methods judiciously

## Testing Guidelines

### Test-Driven Development
- Write tests before implementation when appropriate
- Focus on behavior, not implementation details
- Use descriptive test method names that explain the scenario
- Example: `shouldRejectOrderWhenInventoryIsInsufficient()`

### Test Isolation
- Each test should be independent and self-contained
- Use setup methods carefully to avoid coupling between tests
- Mock external dependencies that are not under test
- Test one behavior per test method

### Testing Pyramid
- More unit tests than integration tests
- More integration tests than end-to-end tests
- Focus unit tests on business logic, not infrastructure
- Use appropriate testing tools for each layer
