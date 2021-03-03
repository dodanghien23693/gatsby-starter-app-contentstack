## Live Demo

You can check out the live demo to get a first-hand experience of the website.

## Prerequisites

Install nodejs on your system.
Install Gatsby CLI.
npm install -g gatsby-cli

## Clone the repo

Clone the following repo. It contains all the required dependencies.
git clone https://github.com/contentstack/gatsby-starter-app-contentstack.git gatsby-starter

## Install dependencies
Go to the gatsby-starter folder, and run the following:
cd gatsby-starter
npm install
This downloads the required files and initializes the site.

## Update Contentstack secrets

In Contentstack, navigate to the Tokens section by clicking on the Settings icon in the menu and clicking on “Tokens”. Click on “development token”
Open .env.development and update with your Contentstack details, including your API key and delivery token.
It should end up looking something like:
CONTENTSTACK_API_KEY=blt...
CONTENTSTACK_DELIVERY_TOKEN=cs...
CONTENTSTACK_ENVIRONMENT=development
CONTENTSTACK_HOSTED_URL='https://gatsby-cdn.contentstack.com/v3'
CONTENTSTACK_CDN=http://localhost:8000/

## Launch Your Gatsby Server

Go to the gatsby-starter folder, and run the following:
cd gatsby-starter
npm run develop 

## Extending the Home Page with a New Component

Part 1 - Create a New Component in Contentstack
In Contentstack, click “CONTENT” to access the list of Content Typesin the
Hover over the “Page” content type, click on the 3 dots to the right side and select “Edit Content Type”.
Navigate to the “Page Components” and scroll to the block titled “Custom Section”, it will be at the end (for your convenience we’ve prebuilt most of this component)
From the left side, drag the “Multi Line Textbox” field type and place it IN THE MODULAR BLOCK. Set the Display name as “Description”. The Unique ID will automatically set itself as “description”. Please make sure there are no spelling mistakes for everything to work well.
From the left side, drag a “File” field type and place it IN THE MODULAR BLOCK. Set the Display Name as “Page Component”. The Unique ID will automatically set itself as “page_components”. Please make sure there are no spelling mistakes for everything to work well.
Click “Save and Close”
Part 2 - Add the Content to Your Page
Click on “Page”
Select the “Home” entry
Scroll to the “Page Components” and click on the “Custom Section” button
Enter the following content:
Title H2: A brand new custom section
Title H3: A brand new react component
Description: Here is a new description
Image: 
Click on “Choose from uploads”
Click on the “Homepage” folder
Select “image.svg”
Image Alignment will default to “Left”
Call to Action: CTA
URL: https://contentstack.com/ 
Publish the update to your entry:
Click “Publish” in the bottom bar
Select “development”
Click “Publish”
Part 3 - Add Code to Render the Component
Update GraphQL Query to Fetch the “Custom Section” Modular Block
Add a new GraphQL query section to fetch content from our newly created Custom Section data from Contentstack.
Go back to your code editor and navigate to src > pages > index.js
Scroll to line 159 and add paste the following and save the updated file:

custom_section {
    title_h2
    title_h3
    description
    image {
        title
        url
    }
    image_alignment
    call_to_action {
        title
        href
    }
}


Create the React Component for the “Custom Section” Modular Block
Navigate to src > components
Create a new file in your components folder called CustomSection.js.
Copy and paste the following into the file and save it.
import { Link } from "gatsby"
import React from "react"
const CustomSection = ({ data }) => {
  function contentSection(data) {
    return (
      <div className="home-content">
        {data.custom_section.title_h2 && <h2>{data.custom_section.title_h2}</h2>}
        {data.custom_section.title_h3 && <h3>{data.custom_section.title_h3}</h3>}
        {data.custom_section.description && <p>{data.custom_section.description}</p>}
        {data.custom_section.call_to_action.title &&
        data.custom_section.call_to_action.href ? (
          <Link
            to={data.custom_section.call_to_action.href}
            className="btn secondary-btn"
          >
            {data.custom_section.call_to_action.title}
          </Link>
        ) : (
          ""
        )}
      </div>
    )
  }
  function imageContent(data) {
    return (
      <img src={data.custom_section.image.url} alt={data.custom_section.image.filename} />
    )
  }
  return (
    <div className="home-advisor-section">
      {data.custom_section.image_alignment === "Left"
        ? [imageContent(data), contentSection(data)]
        : [contentSection(data), imageContent(data)]}
    </div>
  )
}
export default CustomSection

Rendering the “Custom Section” Modular Block

Navigate back to our components folder (src > components) and open RenderComponents.js. We will now add the new CustomSection React component to our page.
On line 12, insert the following text: 	
import CustomSection from "./CustomSection"
On line 30, insert the following code:
if (component["custom_section"]) {
   return <CustomSection data={component} key={index} />
}

Save the file
Re-compile Your Gatsby App
Note: if your Gatsby server is already running, it will likely automatically re-compile your work. If you get an error, please try recompiling your app and double check the above steps (especially spelling!)

In your Terminal, type “npm run develop”.
Your homepage in your web browser should automatically refresh itself.


Can we add this part to the readme:

Launch Your Gatsby Server
Go to the gatsby-starter folder, and run the following:
cd gatsby-starter
npm run develop

Can we get this doc added to the repository:

https://docs.google.com/document/d/1pNsrYK4AaCubBYH2bhA7_OzAH5P04TXfz8NBLTdhlRg/edit?ts=603efcae#

Can we get code changes, that have comment lines for where to insert code snippets in the files themselves.

index.js 
Line 159: // << insert code here >>
Rendercomponents.js 
Line 12: // << insert code here >>
Line 30: // << insert code here >>