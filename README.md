# CS307 2022Fall Project2 Q\&A

## Questions and Answers

1. Exactly which APIs change the item states?
  - There are several questions about the details of the state transitions. Most of them are explained here:
    1. At beginning, a courier invokes `newItem` API with a new `ItemInfo`. Only the EMPTYable fields mentioned in the documentation and the `ItemState` can be `null`. Note that the input `ItemInfo` shall have different import/export and retrieval/deliverty cities and correct import/export taxes corresponding to the tax rates.
    2. At `Picking-up`, only the item's retrieval courier can invoke `setItemState` API to set its state to `To-Export Transporting`.
    3. At `To-Export Transporting`, only the item's retrieval courier can invoke `setItemState` API to set its state to `Export Checking`.
    4. At current state, the invocation of `getItemsAtPort` to the item's export city shall return an array containing this item.
    5. At `Export Checking`, there is only an API `setItemCheckState` which can set `Export Checking` to `Packing to Container` or `Export Check Fail`. If the item's state is set to `Packing to Container` via **this API**, its `Container Code` and `Container Type` will be empty.
    6. At `Packing to Container`, there is an API `loadItemToContainer` which can set `Packing to Container` to `Packing to Container` with a side effect that sets the empty `Container Code` and `Container Type` to the API input parameter. This state transition is represented by the dotted line in the figure below.
    7. At current state, i.e., the `Container Code` is not empty, API `loadItemToContainer` can still be invoked to change the `Container Code` and/or `Container Type`. As noted in the documentation, one container can contains at most one item.
    8. At `Packing to Container`, there is also an API `loadContainerToShip` which can set `Packing to Container` to `Waiting for Shipping` if neither the item's `Container Code` nor its `Container Type` is empty.
    9. Only `shipStartSailing` can set `Waiting for Shipping` to `Shipping`.
    10. Only `unloadItem` can set `Shipping` to `Unpacking from Container`.
    11. Only `itemWaitForChecking` can set `Unpacking from Container` to `Import Checking`.
    12. At current state, the invocation of `getItemsAtPort` to the item's import city shall return an array containing this item.
    13. At `Import Checking`, there is only an API `setItemCheckState` which can set `Import Checking` to `From-Import Transporting` or `Import Check Fail`. If the item's state is set to `From-Import Transporting` via **this API**, its `Delivery Courier` will be empty.
    14. At `From-Import Transporting`, if the `Delivery Courier` is empty, any courier working at the items `Delivery City` can invoke `setItemState` API to set its state from `From-Import Transporting` to `From-Import Transporting` with a side effect that sets the `Delivery Courier` to his/her name.  This state transition is represented by the dotted line in the figure below.
    15. At current state, i.e., the `Delivery Courier` is not empty, only the item's `Delivery Courier` can invoke `setItemState` API to set item's state from `From-Import Transporting` to later ones.
![Detailed State Flow](updated_state_flow.jpg)
  - If the current local judge returns otherwise, please refer to this Q\&A supplement since it will be updated more often.
  - Most of the details in this answer are mentioned in the documentation, however, they are separated at different APIs on different pages. We apologize for the additional comprehension obstacles.
  
2. ~~Which items shall the API `getItemsAtPort` return?~~
  - ~~As described in the documentation: "items currently waiting at this officer’s working seaport".~~
  - ~~We apologize for the bug in the local judge of this API, which will be fixed soon.~~

3. Is there any time-related constraints?
  - No. Otherwise this project would be too complicated.

4. Will there be new items, containers, ships and/or couriers?
  - Yes for items, no for other.
  - There may be couriers not mentioned in `records.csv`, however, they must show in `staffs.csv`.
  
5. How do we check the permission management?
  - By a **simple** permission test and the your uploaded permission granting statements.
  - Hence, there are a series of ways of implementing the permission management, you can choose a way you desire.

6. Can one loads an empty container or sails an empty ship?
  - No. Since there will be no states stored explicitly for the containers and ships in a minimal implementation, such operations would result in no change in such implementation.

7. How can one obtain a shipping or occupying status of a ship or a container?
  - Via SQL statements that look up for item(s) of `Shipping` state with given ship or items of `Packing to Container`--`Unpacking from Container` states with given container.

8. Can one loads a contrainer to a sailing ship?
  - *To be confirmed*
  
9. Are the passwords directly passed to the APIs and obtained from them?
  - Yes. It is not permitted in real-world scenario, however, it is simpler for a course project.
  
10. What are the main contains of the report?
  - First, the overall structure of your modified database design.
  - Then, the permission management statements.
  - Most importantly, which SQL statements you have used to implement these APIs.
  - For groups that complete the advanced requirements, additional contents of the advanced requirements shall also be included.
