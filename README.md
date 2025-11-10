# Zen Stones Website

## Introduction

- This is a catalog website built with Next.js App Router.
- Built and deployed using common practices described in the details section.
- The core idea was to deliver a scalable website featuring at first only a catalog of products and an admin space to manage products and the product landing page.

## Details

- ### Main Page:
  Main page or the product landing page includes a hero with six images for wide screens (4 for smaller displays), 2 main featured items and a section including a collection of different products chosen directly by the admin.

- ### Catalog Page:
  This page is where the user can filter whatever it needs to browse the items they want and also be able to click into the details of a product they like.

- ### Product Page:
  In this page the user will see all the details included in the product and it will be able to also inquiry about it.

- ### Inquiry Page:
  Where the user can inquiry about anything or specially the products if redirect from a product page.

- ### Admin Space Page:
  Where admin can have manage the products, types and the main page.

## Code (under the hood)

- ### Inquiry: 
  The first complex logic the user can get in touch with right away is the **inquiry option** which lets them email the store owner about any concern or general inquiry.

  1. Implemented with **Resend** and **React Emails**.
  2. In the `InquiryForm()` component, using server actions, the program calls the `sendInquiry()` function upon submit.
  3. **Zod** validates the input fields through `inquirySchema`.
  4. `sendInquiry()` calls the api route `/api/inquiry` with a POST method and sends over  some info from the form with it.
  5. Inside the API route, **Resend** handles the email delivery and receives a React component `InquiryEmail()` as the email content.

  #### Function references:
  - `InquiryForm()` → `/app/ui/inquiry/InquiryForm.tsx`
  - `sendInquiry()` → `/app/lib/actions.ts` → line 342
  - `InquiryEmail` → `app/emails/inquiry.tsx`
  - `inquirySchema` → `app/lib/schemas.ts` → line 115
  <br></br>
  ---

- ### Log In:
  The login function is only included for the admin in this version which gets it access to the admin space and power to manage the main page, products and types.

  1. In the `LoginForm()` component, using server actions, the program calls the `login()` function upon submit.
  2. **Zod** validates the input fileds through `loginSchema`.
  3. `login()` will try to fetch user from the database by checking the password and email credentials.
  4. If success in the last step, we create a session for the user with the `createSession()` function using a **JWT token** provided by **jose**.
  5. User gets redirect to the main page.

  #### Function references:
  - `LoginForm()` → `/app/ui/LoginForm.tsx`
  - `login()` → `/app/lib/actions.ts` → line 216
  - `createSession` → `app/lib/session.ts` → line 9
  - `loginSchema` → `app/lib/schemas.ts` → line 30
  <br></br>
  ---

- ### Log Out:
  If the log out button is clicked the user is logged out.

  1. In the `Log()` component, the function `logout()` is called if the admin is logged in.
  2. Then `deleteSession()` is fired deleting the cookie session for the admin.

  #### Function references:
  - `Log()` → `/app/ui/inquiry/Log.tsx`
  - `logout()` → `/app/lib/actions.ts` → line 478
  - `deleteSession()` → `/app/lib/session.ts` → line 23
  <br></br>
  ---

- ### Product CRUD
  Logic that rules the product management where the admin can create, read, update or delete items.

  ### Creating a product:
   1. In the `AddProductForm()` component, using server actions, the program calls the `createProduct()` function upon submit.
   2. **Zod** validates the input fileds through `productSchema`.
   3. Calls the **AWS S3** API thorugh the `callS3API()` function to upload the product images to a **S3 bucket**.
   4. Inserts all the product information retrieved from the form to the database.
   5. With the returning image url from the S3 API call and the id from the inserted product, the image info is inserted in its own table in the database.
   6. Lastly, the function will update the URL so `FeedbackDialog()` component can display a feedback bubble stating the successful product creation.

  ### Updating a product:
    1.

  ### Deleting a product:
    1.

  ### Reading a product:
    1.

  #### Function references:
  - `AddProductForm()` → `/app/ui/adminspace/AddProductForm.tsx`
  - `createProduct()` → `/app/lib/actions.ts` → line 46
  - `productSchema()` → `/app/lib/schemas.ts` → line 40
  - `callS3API()` → `/app/lib/actions.ts` → line 567
  - `FeedbackDialog()` → `/app/ui/adminspace/FeedbackDialog.tsx`
  <br></br>
  ---