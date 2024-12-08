# e-commerce-site

Milestone 3: Project
Objective: Build a basic e-commerce website using Next.js with features such as a products listing, product details page, a shopping cart, and API routes for handling product data.

Steps to Implement
Set Up the Project
Create API Routes for Products
Build the Products Listing Page
Create Product Details Page
Implement the Shopping Cart
Deploy the Project
Step 1: Set Up the Project
Start by creating a new Next.js project:

bash
Copy code
npx create-next-app@latest e-commerce
cd e-commerce
Step 2: Create API Routes for Products
We will create an API route to handle products data.

Create a file pages/api/products.js to return a list of products.

javascript
Copy code
export default function handler(req, res) {
  const products = [
    { id: '1', name: 'T-shirt', price: 20, description: 'High-quality cotton t-shirt', image: '/tshirt.jpg' },
    { id: '2', name: 'Sneakers', price: 50, description: 'Comfortable running sneakers', image: '/sneakers.jpg' },
    { id: '3', name: 'Backpack', price: 35, description: 'Durable backpack for all your needs', image: '/backpack.jpg' },
  ];
  res.status(200).json(products);
}
Step 3: Build the Products Listing Page
The homepage will display a list of products fetched from the API route.

Create a file pages/index.js to list products:

javascript
Copy code
import { useEffect, useState } from 'react';
import Link from 'next/link';

export default function Home() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    fetch('/api/products')
      .then((res) => res.json())
      .then((data) => setProducts(data));
  }, []);

  return (
    <div className="p-4">
      <h1 className="text-3xl font-bold mb-4">Products</h1>
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        {products.map((product) => (
          <div key={product.id} className="border p-4 rounded">
            <img src={product.image} alt={product.name} className="w-full h-40 object-cover mb-2" />
            <h2 className="text-xl font-semibold">{product.name}</h2>
            <p className="mb-2">Price: ${product.price}</p>
            <Link href={`/product/${product.id}`} className="text-blue-600 underline">
              View Details
            </Link>
          </div>
        ))}
      </div>
    </div>
  );
}
Step 4: Create Product Details Page
Each product will have a dedicated page showing detailed information. We will create a dynamic route to fetch the details of a specific product.

Create a file pages/product/[id].js:

javascript
Copy code
import { useRouter } from 'next/router';
import { useState, useEffect } from 'react';

export default function ProductDetails() {
  const router = useRouter();
  const { id } = router.query;
  const [product, setProduct] = useState(null);

  useEffect(() => {
    if (id) {
      fetch('/api/products')
        .then((res) => res.json())
        .then((data) => setProduct(data.find((item) => item.id === id)));
    }
  }, [id]);

  if (!product) return <p>Loading...</p>;

  return (
    <div className="p-4">
      <h1 className="text-3xl font-bold mb-4">{product.name}</h1>
      <img src={product.image} alt={product.name} className="w-full h-64 object-cover mb-4" />
      <p>{product.description}</p>
      <p className="mt-2">Price: ${product.price}</p>
    </div>
  );
}
Step 5: Implement the Shopping Cart
We’ll use React state to manage the shopping cart and display added items.

Create a components/Cart.js for displaying the cart:

javascript
Copy code
import { useState } from 'react';

export default function Cart({ cartItems, setCartItems }) {
  const removeItem = (id) => {
    setCartItems(cartItems.filter((item) => item.id !== id));
  };

  const totalPrice = cartItems.reduce((acc, item) => acc + item.price, 0);

  return (
    <div className="p-4 border mt-4">
      <h2 className="text-xl font-bold mb-4">Shopping Cart</h2>
      {cartItems.length === 0 ? (
        <p>Your cart is empty.</p>
      ) : (
        <>
          <ul>
            {cartItems.map((item) => (
              <li key={item.id} className="flex justify-between py-2">
                <span>{item.name}</span>
                <button onClick={() => removeItem(item.id)} className="text-red-600">Remove</button>
              </li>
            ))}
          </ul>
          <p className="mt-4">Total: ${totalPrice}</p>
        </>
      )}
    </div>
  );
}
In the pages/index.js, integrate the shopping cart logic:

javascript
Copy code
import { useState, useEffect } from 'react';
import Cart from '../components/Cart';

export default function Home() {
  const [products, setProducts] = useState([]);
  const [cartItems, setCartItems] = useState([]);

  useEffect(() => {
    fetch('/api/products')
      .then((res) => res.json())
      .then((data) => setProducts(data));
  }, []);

  const addToCart = (product) => {
    setCartItems([...cartItems, product]);
  };

  return (
    <div className="p-4">
      <Cart cartItems={cartItems} setCartItems={setCartItems} />
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        {products.map((product) => (
          <div key={product.id} className="border p-4 rounded">
            <img src={product.image} alt={product.name} className="w-full h-40 object-cover mb-2" />
            <h2 className="text-xl font-semibold">{product.name}</h2>
            <p className="mb-2">Price: ${product.price}</p>
            <button onClick={() => addToCart(product)} className="bg-blue-600 text-white px-4 py-2 rounded">
              Add to Cart
            </button>
          </div>
        ))}
      </div>
    </div>
  );
}
Step 6: Deploy the Project
Push your code to a GitHub repository.
Go to Vercel, create an account (if you don’t have one), and link your GitHub repository.
Vercel will automatically deploy your project and provide a live URL.
Project Features
Product Listing: Shows all products fetched from an API route.
Product Details: Displays detailed information for a specific product.
Shopping Cart: Allows adding/removing products, and displays total price.
