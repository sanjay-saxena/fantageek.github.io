---
author: onmyway133
comments: true
date: 2013-10-14 10:22:52+00:00
layout: post
slug: implementation-of-property-in-ios
title: Implementation of property in iOS
wordpress_id: 606
categories:
- iOS
tags:
- assign
- copy
- implement
- ios
- property
- retain
---

For better understanding of retain, assign, copy keyword used in Property declaration, it's best to look at how they are implemented




**assign**




[code language="objc"]  

@property (nonatomic, assign) NSInteger count;  

[/code]




"assign" is the default. In the setter that is created by @synthesize, the value will simply be assigned to the attribute. My understanding is that "assign" should be used for non-pointer attributes.  

<!-- more -->




[code language="objc"]  

//getter  

- (NSInteger)count  

{  

    return count;  

}




//setter  

- (void)setCount:(NSInteger)aCount  

{  

    count = aCount;  

}  

[/code]




**retain**




[code language="objc"]  

@property (nonatomic, retain) Book *book;  

[/code]




"retain" is needed when the attribute is a pointer to an object. The setter generated by @synthesize will retain (aka add a retain count) the object. You will need to release the object when you are finished with it.




[code language="objc"]  

//getter  

- (Book *)book  

{  

    return book;  

}




//setter  

- (void)setBook:(Book *)aBook  

{  

    if (book == aBook)  

    {  

        return;  

    }  

    Book *oldBook = book;  

    book = [aBook retain];  

    [oldBook release];  

}  

[/code]




**copy**




[code language="objc"]  

@property (nonatomic, copy) Book *book;  

[/code]




"copy" is needed when the object is mutable. Use this if you need the value of the object as it is at this moment, and you don't want that value to reflect any changes made by other owners of the object. You will need to release the object when you are finished with it because you are retaining the copy.




[code language="objc"]  

//getter  

- (Book *)book  

{  

    return book;  

}




//setter  

- (void)setBook:(Book *)aBook  

{  

    if (book == aBook)  

    {  

        return;  

    }  

    Book *oldBook = book;  

    book = [aBook copy];  

    [oldBook release];  

}  

[/code]
