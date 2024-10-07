---
title: 'Creating a Dynamic Component with CSS Variables in React'
description: 'In this post, we’ll build a reusable Card component in React that uses CSS variables for customizable styles. Learn how to easily pass colors as props, enhancing both flexibility and maintainability in your UI design'
publishDate: '04 June 2024'
tags: ['react', 'css variables', 'tips', 'english']
---

## Creating a Dynamic Component with CSS Variables in React

In this blog post, we’ll explore how to build a reusable Card component in React that leverages CSS variables for customizable styling. You'll learn how to pass different colors as props, ensuring that each card can have its unique look while maintaining a consistent design. This approach enhances both the flexibility and maintainability of your UI components. Whether you're building a dashboard or a simple display, this dynamic card will add flair to your application.

Card.tsx

```tsx
function Card({ title, description, color = 'blue' }: CardProps) {
	return (
		<div className='card' style={{ '--cardColor': color } as CSSProperties}>
			<h3 className='card-title'>{title}</h3>
			<p>{description}</p>
		</div>
	)
}
```

Card.scss

```scss
.card {
	--cardColor: blue; // default color

	border: 1px solid var(--cardColor);
	padding: 16px;
	border-radius: 8px;
	transition: border-color 0.3s ease;

	&-title {
		color: var(--cardColor);
		margin-bottom: 8px;
	}
}
```

## Explanation for the Blog Post

1. **CSS Variables**

   - The component uses CSS variables to set the color of the card. This allows for easy customization of colors directly from the component's props.
   - By default, the card will be blue if no color is passed.

2. **Reusable Component**

   - The **Card** component is reusable. You can create multiple instances of it with different colors without needing to define new classes for each color.

3. **Usage Example**: You can use the Card component in your application like this:

```tsx
<section>
	<Card title='Info Card' description='This is an information card.' color='lightgreen' />
	<Card title='Warning Card' description='This is a warning card.' color='orange' />
	<Card title='Success Card' description='This is a success card.' color='lightblue' />
</section>
```

4. Benefits of This Approach
   - **Dynamic Styles**: Using CSS variables allows for dynamic styling based on props, which enhances the component's flexibility
   - **Maintainability**: By utilizing a single class with CSS variables, you reduce the need for multiple CSS classes, making it easier to maintain your styles.

## Summary

In this blog post, we explored various methods for implementing CSS variables in a React `Card` component, allowing for dynamic styling based on props.

1. **Inline Styles:** We demonstrated how to set CSS variables directly in the JSX using the `style` attribute, enabling quick customization of the card's color.

2. **Styled Components:** We utilized the Styled Components library to define CSS variables within styled components, promoting a modular approach to styling.

3. **CSS Modules:** We showcased how to use CSS Modules to manage styles separately while still incorporating CSS variables for flexibility in design.

By leveraging these techniques, you can create reusable and easily customizable components in your React applications, enhancing both maintainability and visual appeal. Choose the method that best suits your project’s needs to achieve dynamic and responsive designs.
