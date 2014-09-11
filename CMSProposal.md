# ELIS 5.1 Content Management 

## Overview
This document describes the design an pattern for Content Management using MongoDB.

## Problem
The current ELIS2 application utilizes ORACLE  Blobs to store documents and images associated with
a case.  Additional table structures are defined to support meta data pertaining to the images and documents stored as Blobs.


## Solution
To support this solution we will use MongoDB with a flexible schema to store all content “nodes” in a single collection regardless of type.  This document will provide prototype schema for the following “node” types.

In the ELIS2 domain content nodes are typically evidence(s) associated with case, these evidences are:

## Documents and Images
Documents and Images can be associated to a gallery and store title, description, author, and date along with the and other ELIS specific meta data as well as actual  binary data.

In the future nodes such as 
###Basic Page
Basic pages are useful for displaying infrequently-changing text such as an ‘about’ page. With a basic page, the salient information is the title and the content.

May also be supported through these schemas. 

## Schema
Although documents/images  in the collection contain content of different types, all documents have a similar structure and a set of common fields. 
GridFS provides the ability to store larger files in MongoDB. GridFS stores data in two collections, in this case, **eliscms.assets.files**, which stores metadata, and **eliscms.assets.chunks** which stores the data itself. Consider the following prototype document from the **eliscms.assets.files** collection:


```
{
    _id: ObjectId(...),
    length: 123...,
    chunkSize: 262144
    uploadDate: ISODate(...),
    contentType: 'image/jpeg',
    metadata: {
          nonce: ObjectId(...),
          slug: '2014-04-fingerprint-scan',
          type: 'photo',
          section: 'my-case',
          title: 'Fpkitten',
          created: ISODate(...),
          author: {_id: ObjectId(...), name: 'Lynnetta'},
          tags: […], 
          detail: {
                 filename: 'Fpkitten_fingerprint_scan.jpg',
                 resolution: [1600, 1600], …}
         }
}
```

