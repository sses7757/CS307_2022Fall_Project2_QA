# CS307 2022Fall Project2 Q\&A

## Questions and Answers

- Exactly which APIs change the item states?
  - There are several questions about the details of the state transitions. Most of them are explained here:
    1. At beginning, a courier invokes `newItem` API with a new `ItemInfo`. Only the EMPTYable fields mentioned in the documentation can be `null`. Note that the inpu parameter given by the judge **may or may not** have redundant fields like the item's current state (must be `Picking-up`) and the retrieval courier (must be the `LogInfo` user).
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

- Which items shall the API `getItemsAtPort` return?
  - As described in the documentation: "items currently waiting at this officer’s working seaport".
  - We apologize for the bug in the local judge of this API, which will be fixed soon.

- Is there any time-related constraints?
  - No. Otherwise this project would be too complicated.

