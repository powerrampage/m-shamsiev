---
title: 'Conditional Rendering in React'
description: 'In this post, learn how to use some conditional rendering in React. Simplify your JSX logic, making your code more readable and maintainable.'
publishDate: '07 October 2024'
tags: ['react', 'JSX', 'IIFE', 'tips', 'pojo', 'english']
---

## Ternary Operator

```jsx
<article className='card'>
	{status === 'loading' ? (
		<p>Loading...</p>
	) : status === 'success' ? (
		data.length ? (
			<p>Data loaded successfully!</p>
		) : (
			<p>Data is empty!</p>
		)
	) : status === 'error' ? (
		<p>Error loading data</p>
	) : (
		<p>Status unknown</p>
	)}
</article>
```

Condition with ternary operators, the readability decreases, and maintaining the logic becomes challenging.

## Logical AND (&&) Operator

```jsx
<article className='card'>
	{status === 'loading' && <p>Loading...</p>}
	{status === 'success' && data.length ? <p>Data loaded successfully!</p> : <p>Data is empty!</p>}
	{status === 'error' && <p>Error loading data</p>}
	{!status && <p>Status unknown</p>} // else ??
</article>
```

The logical && operator only renders the element if the condition is true, but there is no direct way to handle the "else" case in this structure. As a result, in situations where you want to render something when the condition is false, you'll need to write separate conditions, leading to redundancy.

## POJO (Plain Old JS Object) :D

```jsx
const statusMessages = {
	loading: <p>Loading...</p>,
	success: data.length ? <p>Data loaded successfully!</p> : <p>Data is empty!</p>,
	error: <p>Error loading data</p>,
	unknown: <p>Status unknown</p> // Fallback for undefined or unknown status
}

<article className='card'>{statusMessages[status] || statusMessages.unknown}</article>
```

Pros:

Improved Readability - Easier to Manage and Scale - Eliminates Redundant Checks - Eliminates Redundant Checks - Clear Fallback (Else Handling) - Flexibility

Cons:

- Limited Logic in Object

  - While simple conditions (like checking `data.length`) can be handled within the object, more complex logic may still need to be handled outside the POJO. For very dynamic or intricate conditions, a pure object may not suffice

- Potential Overhead for Complex Scenarios
  - If you have many dynamic conditions that depend on multiple variables, you may still need additional logic to populate the POJO values dynamically, which could make the solution less effective for very complex scenarios

## IIFE Conditional Rendering

```jsx
<article className='card'>
	{(() => {
		if (status === 'loading') return <p>Loading...</p>
		if (status === 'error') return <p>Error loading data</p>
		if (status === 'success') {
			if (data.length) return <p>Data loaded successfully!</p>
			else return <p>Data is empty!</p>
		}
		return <p>Status unknown</p>
	})()}
</article>
```

These examples show how IIFEs provide flexibility for embedding logic directly into JSX while maintaining clear and concise code

Cons:

- Performance Overhead
  - IIFEs create a new function every time the component renders, which may cause unnecessary performance overhead, particularly in performance-sensitive applications or components that render frequently
- Inconsistent with Common React Patterns
  - IIFEs aren't widely used or recommended in React

Pros:

- Encapsulation of Logic
  - IIFEs allow you to encapsulate conditional logic directly within JSX
- Inline Evaluation
  - Since IIFEs are immediately invoked, they are useful for dynamically evaluating conditions on the fly, especially if those conditions are complex and involve multiple variables
- Flexibility in JSX
  - You can use any JavaScript inside an IIFE, providing flexibility for more complex logic and operations, such as loops or multiple conditions, without needing to move logic outside JSX

## Final Consideration

- For **simple conditions**, stick to **ternary operators** or **logical &&**.
- For **multiple states**, consider using if-else, switch-case, or even **POJO** mappings.
- Use **IIFEs** sparingly, as they can hurt readability and performance in complex components.

The key is to **balance readability** and maintainability while keeping performance in mind as your component grows.
