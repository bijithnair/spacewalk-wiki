# Multiorg Phase II: Content Sharing

## UI Mockups


### The Sharer




 * under custom channel details: radio button: private (default), public, public only to these trusted orgs: [ ] 
   * turns out there is already per-user in-org subscriber ACLs here, so I reimagined the main custom channel details page and added the org sharing controls to a new access control section. We'll also add a new tab to check on and off org's access to the channel. 
 * list of orgs to check on/off for access to custom channel (new 'organization' tab in the custom channel details screens) 
 * new channel filters: 
   * my channels i'm sharing 
   * my private channels
#### Re-arranged custom channel details tab with additional organization sharing controls



 * *NOTE:* new support fields will not appear on Red-Hat-provided channels (Satellite only)

![Alt](images/channel-details-sharing.png?raw=True)

===== Confirmation to go from public to protected =====
![Alt](images/channel-details-orgs_protectedconfirm.png?raw=True)

===== Confirmation to go from public or protected to private =====
![Alt](images/channel-details-orgs_privateconfirm.png?raw=True)
#### Per-organization access control for custom channel (trusted orgs only)



![Alt](images/channel-details-orgs.png?raw=True)

===== Confirm screen to remove external org access to your channel =====
![Alt](images/channel-details-orgs_removalconfirm.png?raw=True)
### The Consumer



 * new channel filter 
   * shared channels from other orgs
#### New channel lists filters to make it easier to sort shared channels out from other channels

 * *All Channels*: All channels, all arches, all sources, regardless of how many systems subscribed, regardless of whether or not it has been retired, regardless if it comes from another org or is local to your org

   * Columns: channel name, #packages, #systems subscribed, source/provider
 * *Popular Channels*: Will have a drop-down at the top of the pages, 'View Channels with at least [ __ ] Systems subscribed' and the dropdown should be 1 | 5 | 10 | 25 | 50 | 100 | 500 | 1000
   * Columns: channel name, #packages, #systems subscribed, source/provider
 * *Red Hat channels*: All arches of all non-retired Red Hat provided channels (will only exist in Satellite; doesn't make sense in Spacewalk)
   * Columns: channel name, #packages, #systems subscribed
 * *My Channels*: All custom channels that are local to this organization (would be nice to have tabs across the top: Private | Protected | Shared)
   * Columns: channel name, #packages, #systems subscribed
 * *Shared channels* (Rename to *External Channels*?): All channels that have been provided by an external org
   * Columns: channel name, #packages, #systems subscribed, source/provider
 * *Retired Channels*: channels that have been retired
   * Columns: channel name, #packages, #systems subscribed, source/provider

![Alt](images/channels-list-new-filters.2.png?raw=True)
#### Reorganized channel details page includes contact/support information



![Alt](images/channel-details-consumer.png?raw=True)

question: difference between standard IT build channel and apps channels?

