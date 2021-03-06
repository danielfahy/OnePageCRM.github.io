---
layout: api
title: "Request Parameters"
slug: "requestparams"
category: basics
tags: beta
date: 2014-02-19 14:05:55

---

<p>You can configure your requests with standard HTTP parameters.<br/>Just add them to HTTP query string for any request:</p>
<p class="url">
  <code class="bluetext">
    <span class="get-text">GET</span> contacts.json?per_page=10&page=2
  </code>
</p>
<p>Or as part of HTTP request body for <span class="post-text">POST</span> and <span class="put-text">PUT</span> requests.</p>
<p>This section outlines the main parameters. To see all parameters supported by a particular resource, please visit that resource’s section.</p>

<h3 id="paginating-responses">Paginating Responses <span class="get-text indexup">GET</span></h3>
<p>Whenever you are fetching a list of records, you can paginate the response with these standard parameters:</p>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="nowrap"><code class="redtext">integer</code></td>
      <td class="nowrap"><code class="bluetext">page</code></td>
      <td>Page number. Starts from 1. Default is 1.</td>
    </tr>
    <tr>
      <td><code class="redtext">integer</code></td>
      <td><code class="bluetext">per_page</code></td>
      <td>Number of records to return. Maximum 100 allowed. Default is 10.</td>
    </tr>
  </tbody>
</table>
<p>So, for example, to fetch first batch of 20 contacts:</p>
<p class="url"><code class="bluetext"><span class="get-text">GET</span> contacts.json?per_page=20&page=1</code></p>
<p>To fetch second batch of 20 contacts, you would use:</p>
<p class="url"><code class="bluetext"><span class="get-text">GET</span> contacts.json?per_page=20&page=2</code></p>

<h3 id="specifying-the-fields-you-need">Specifying the Fields You Need <span class="get-text indexup">GET</span></h3>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="nowrap"><code class="redtext">string</code></td>
      <td class="nowrap"><code class="bluetext">fields</code></td>
      <td>Comma-separated lists of fields to return for each record.</td>
    </tr>
  </tbody>
</table>
<p><code class="bluetext">fields </code>parameter allows you to exclude data you don’t need from response, include additional data that is not returned by default, and/or include associated resources to the response.<br/>
  For detailed description of this parameter, see <a href="fields.html">Fields</a> section.</p>

<h3 id="undoing-delete">Undoing a Delete <span class="delete-text indexup">DELETE</span></h3>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="nowrap"><code class="redtext">boolean</code></td>
      <td class="nowrap"><code class="bluetext">undo</code></td>
      <td>Revert the deleting operation.</td>
    </tr>
  </tbody>
</table>
<p>If you delete a contact or a tag with a <span class="delete-text">DELETE</span> request like this:</p>
<p class="url"><code class="bluetext"><span class="delete-text">DELETE</span> contacts/51f251d5eb899749f7000006.json</code></p>
<p>You can undo it by specifying an undo flag:</p>
<p class="url"><code class="bluetext"><span class="delete-text">DELETE</span> contacts/51f251d5eb899749f7000006.json?undo=1</code></p>

<h3 id="switching-active-user">Switching Active User <span class="get-text indexup">GET</span> <span class="post-text indexup">POST</span> <span class="put-text indexup">PUT</span></h3>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="nowrap"><code class="redtext">string</code></td>
      <td class="nowrap"><code class="bluetext">user_id</code></td>
      <td>ID of a user on logged in user’s team.</td>
    </tr>
  </tbody>
</table>
<p>Whenever you want to do something on behalf of logged in user’s teammate, use <code class="bluetext">user_id</code> parameter.</p>
<p>For example,</p>
<p class="url"><code class="bluetext"><span class="get-text">GET</span> actions.json?user_id=51f251d5eb899749f7000006</code></p>
<p>would get actions for user with specified ID.</p>
<p>User ID specified in this parameter would also be used to populate any fields where otherwise logged in user’s ID would be default. For example, if you create a contact without specifying <code class="bluetext">owner_id</code>, but with <code class="bluetext">user_id</code> provided:</p>
<p class="url"><code class="bluetext"><span class="post-text">POST</span> contacts.json?user_id=51f251d5eb899749f7000006&first_name=Jane&last_name=Doe</code></p>

<h3 id="stripping-empty-fields">Stripping Empty Fields <span class="get-text indexup">GET</span></h3>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="nowrap"><code class="redtext">boolean</code></td>
      <td class="nowrap"><code class="bluetext">sparse</code></td>
      <td>Only include non-empty fields in the response.</td>
    </tr>
  </tbody>
</table>
<p>If you specify the <code class="bluetext">sparse</code> flag, all fields which values are considered empty will be stripped from the response, even if they are specified in the <code class="bluetext">fields</code> parameter.</p>
<p>Values that are considered empty:</p>
<ul>
  <li>Empty string</li>
  <li>Null</li>
  <li>Boolean false</li>
  <li>Empty array</li>
</ul>
<p>Values that are <span class="italic">not</span> considered empty:</p>
<ul>
  <li>Integer or float zero</li>
  <li>Empty hash (a hash for which all fields have been stripped)</li>
</ul>

<h3 id="providing-partial-data">Providing Partial Data <span class="post-text indexup">POST</span> <span class="put-text indexup">PUT</span></h3>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="nowrap"><code class="redtext">boolean</code></td>
      <td class="nowrap"><code class="bluetext">partial</code></td>
      <td>Ignore all missing fields.</td>
    </tr>
  </tbody>
</table>
<p>By default, all <span class="post-text">POST</span> and <span class="put-text">PUT</span> requests require all resource fields to be provided, otherwise throwing an error.<br/> 
With <code class="bluetext">partial</code> flag set, If you don’t provide a field then for <span class="post-text">PUT</span> requests it would be set to its default value (an empty string for most fields), and for <span class="put-text">PUT</span> requests it would remain its current value.</p>
<p>For example the following request would update contact’s name to Jane, leaving all other fields untouched:</p>
<p class="url"><code class="bluetext"><span class="put-text">PUT</span> contacts/51f251d5eb899749f7000006.json?partial=1&first_name=Jane</code></p>
<p>And here is how you can create a contact named Jane Doe with all other fields set to their default values:</p>
<p class="url"><code class="bluetext"><span class="post-text">POST</span> contacts.json?partial=1&first_name=Jane&last_name=Doe</code></p>
<p><span class="strong">Warning:</span> Beware that if you use this option, any fields that you misspell or accidentally leave out from request will be ignored and their values would be lost, without any warnings or errors. <br/>
This is why, starting from version 3 of our API, partial <span class="post-text">POST</span> requests are rejected by default, and this feature is active only if you specifically request it.</p>

<h3 id="limiting-listings-by-time">Limiting Listings By Time <span class="get-text indexup">GET</span></h3>
<p>When fetching collections, you can make them only return records that were modified in a given time range.</p>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code class="redtext">time</code></td>
      <td><code class="bluetext">date_filter</code></td>
      <td>Filter returned data by a particular field when combined with since and until.</td>
    </tr>
    <tr>
      <td><code class="redtext">time</code></td>
      <td><code class="bluetext">since</code></td>
      <td>Return resources with dates in the provided date_filter parameter since this time. Otherwise it will return resources that were modified since this time.</td>
    </tr>
    <tr>
      <td><code class="redtext">time</code></td>
      <td><code class="bluetext">until</code></td>
      <td>Return resources with dates in the provided date_filter parameter until this time. Otherwise it will return resources that were modified until this time.</td>
    </tr>
    <tr>
      <td class="nowrap"><code class="redtext">time</code></td>
      <td class="nowrap"><code class="bluetext">unmodified_since</code></td>
      <td>Return only records that were unmodified since specified time.</td>
    </tr>
    <tr>
      <td><code class="redtext">time</code></td>
      <td><code class="bluetext">modified_since</td>
      <td>Return only records that were modified since specified time.</td>
    </tr>
  </tbody>
</table>

<h3 id="avoiding-data-you-already-have">Avoiding Data You Already Have <span class="get-text indexup">GET</span></h3>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code class="redtext">time</code></td>
      <td><code class="bluetext">if_modified_since</code></td>
      <td>Only return data if it <span class="italic">could</span> have been modified since specified time.</td>
    </tr>
  </tbody>
</table>
<p>This parameter is similar to HTTP “If-Unmodified-Since” header, and will result in HTTP 304 “Not Modified” status returned if the data was not modified since the specified time.</p>
<p>Note that because of the complexity of our data, as well flexibility of API, if data is returned, it does not necessarily mean that it has been changed. If status 304 is returned however - it is guaranteed that the data has <span class="italic">not</span> been changed.</p>

<h3 id="sorting-listings-by-field">Sorting Listings By Field <span class="get-text indexup">GET</span></h3>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code class="redtext">string</code></td>
      <td><code class="bluetext">sort_by</code></td>
      <td>Return records sorted by a particular field.</td>
    </tr>
  </tbody>
</table>

<h3 id="order-listings-by-field">Order Listing By Field <span class="get-text indexup">GET</span></h3>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code class="redtext">string</code></td>
      <td><code class="bluetext">order</code></td>
      <td>Return records in a specified order, ascending or descending.</td>
    </tr>
  </tbody>
</table>

<h3 id="selecting-listings-by-date-field">Selecting Listings By Date Field <span class="get-text indexup">GET</span></h3>
<table class="table table-striped table-bordered">
  <thead>
    <tr>
      <td>Type</td>
      <td>Name</td>
      <td>Description</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code class="redtext">string</code></td>
      <td><code class="bluetext">date_field</code></td>
      <td>Return records where their provided date field falls in a specific time.</td>
    </tr>
  </tbody>
</table>