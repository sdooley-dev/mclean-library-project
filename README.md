# McLean library management project summary

## Here are the 3 main objectives of the McLean Library Management System project:
1. The ability to track multiple copies of the same book title.
2. The ability to track the branch location of a particular book (expect to have multiple branches)
3. The ability to track individual library patrons and see current and previous lending activity.

## Technologies used in the McLean Library Management System
1. **Declarative**
> Example: Roll-up summary field on Book__c

2. **Visualforce**
> Example: Release notes/readme page

3. **Lightning Web Component**
> Example: LWC on Book Loan record page

4. **Apex Trigger**
> Example: Trigger on insert/update record on Book Copy object.
> Example: Trigger on insert/update record on Book Loan object.

5. **Experience Cloud**
> Example: Catalog look-up on Book object.

> Browse Book copies

> Browse locations - (view map)

6. **Tableau CRM Analytics**
> Example: Analytics on Book object.

7. **Github**
> Example: Uploaded package.xml
> https://github.com/sdooley-dev/mclean-library-project/blob/main/README.md

## Demo
> Login as librarian - View Permissions
### Book object - Declarative - Roll Up Summary Fields
### Create a new Book Title [1]
### Create a new Book Copy (see code below)

## Code for the Book Copy Master Trigger ##

> trigger BookCopyMasterTrigger on Book_Copy__c (    before insert, after insert,
>     before update, after update,
>     before delete, after delete, after undelete) {
> 
>     // check if it is a before event
>     if (trigger.isBefore) {
>         if (trigger.isInsert) {            
>             BookCopyHandlerB.updateBookCopyDetails(trigger.new);   
>         }
> 
>         if (trigger.isUpdate) {
>             // call method here
>         }
> 
>        if (trigger.isDelete) {
>             // call method here
>         }
>     }
> 
>     // check if it is an after event
>     if (trigger.isAfter) {
>         if (trigger.isInsert) {
>             // call method here
>         }
> 
>         if (trigger.isUpdate) {
>             // call method here
>         }
> 
>         if (trigger.isDelete) {
>            // call method here
>        }
> 
>         if (trigger.isUndelete) {
>             // call method here
>         }
>     }
> }

## Code for the Book Copy Handler Class

>  public class BookCopyHandlerB {
> 
>     // method to update Book_Copy_Number_D__c, Loan_Status__c
>     public static void updateBookCopyDetails(List<Book_Copy__c> copies) {
>         Set<Id> bookIds = new Set<Id>();
> 
>        // find the book copy in [Book__c] and add it
>        for (Book_Copy__c copy : copies) {
>           if (copy.Book__c != null) {
>                bookIds.add(copy.Book__c);
>            }
>        }
>
>        Map<Id, Book__c> booksMap = new Map<Id, Book__c>([SELECT Id, Number_of_Copies__c FROM Book__c WHERE Id IN :bookIds]);
>
>        for (Book_Copy__c copy : copies) {
>            if (copy.Book__c != null) {
>                Book__c book = booksMap.get(copy.Book__c);
>
>                //  if a book copy already exist
>                if  ( book.Number_of_Copies__c != null ) {
>
>                    // increment copy number by 1 for that book title
>                    copy.Book_Copy_Number_D__c = book.Number_of_Copies__c + 1;
>                 }
>
>                // if no book copy exist yet, make the copy = 1
>                 if (book.Number_of_Copies__c == null) {
>                    copy.Book_Copy_Number_D__c = 1;
>                 }
>
>                 // insert status
>                 copy.Loan_Status__c = 'Checked In'; 
>
>                // insert date for first check-in
>                copy.Last_Checked_In__c = Date.today(); 
>            }
>        }
>    }
> }

### Create a new Book Loan 
(uses Book Loan Trigger)

## Lightning Web Component for the Book Loan record page
> Custom component uses the `mailto` link

## Utility Bar
> Browse links
> Navigate to Homepage

## Experience Cloud
- Browse catalog.
- Browse book details.
- Browse related.

- Browse Book copies
- Browse locations





## Custom objects used in the data schema
What custom objects are used for the data schema for the McLean Library Salesforce org?
Book__c, Book_Copy__c and Book_Loan__c are the custom objects used in the Salesforce data schema for the McLean Library Management system.

## Standard objects used in the data schema
What standard objects are used for the data schema for the McLean Library Salesforce org?
Account, Contact and User are the standard objects used in the Salesforce data schema for the McLean Library Management system.

## Summary of the book Untamed
The book "Untamed" by Glennon Doyle is currently checked in at the Great Falls Branch.
Summary of the book Untamed: In this memoir, Glennon Doyle shares her journey of self-discovery and empowerment, encouraging readers to embrace their true selves, break free from societal expectations, and live a life guided by their own inner voice.
