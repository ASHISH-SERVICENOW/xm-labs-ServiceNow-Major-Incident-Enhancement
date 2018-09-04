# ServiceNow Major Incident Enhancement
This ServiceNow xMatters Integration enhancement will add ServiceNow Major Incident Management into the xMatters notification process. Accepting a major incident will trigger xMatters notification.


<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

# Pre-Requisites
* ServiceNow Kingston, London
* ServiceNow xMatters v5.0 [How to install xMatters ServiceNow Integration](https://support.xmatters.com/hc/en-us/articles/115004327803)
    * Could be modified to work with older versions of xMatters ServiceNow integration.
* ServiceNow Major Incident Plugin [How to enable ServiceNow Major Incident Plugin](https://docs.servicenow.com/bundle/london-it-service-management/page/product/incident-management/task/activate-major-incident-management-plugin.html)
* xMatters account - If you don't have one, [get one](https://www.xmatters.com)!

# Files
* [xMattersConfig Script Include update XML](ServiceNowUpdates/xMattersConfig-script_include.xml)
* [xMattersIncident Script Include update XML](ServiceNowUpdates/xMattersIncident-script_include.xml)


- If you have not made any customizations to your ServiceNow xMatters integration you can import the above xml file to ServiceNow.

# How it works
xMatters notifications will not be created unless a Major Incident has been _"Accepted"_ in ServiceNow.

Other Major incident statuses, __"Proposed"__ and __"Rejected"__ will not trigger an xMatters notification.

This Major Incident ServiceNow Enhancement will allow one of your teams to review an incident and decide whether it is considered major. At this time if the incident is "Approved", an xMatters notification will be created.

__Important Note:__ This enhancement will stop all notifications from going to xMatters except for __"Approved"__ Major Incidents.
If you still want to send xMatters notifications for other incidents, you may need to make further customizations. The way you implement ServiceNow Major Incident Process could change the way this integration works. See Configuring ServiceNow Major Incident Process section for further details.


# Installation

## Modify __xMatters Incident Insert Business Rule__ script include

1. Search for __"xMattersConfig"__ and open the Script include.

    <kbd>
      <img src="/media/xMattersIncident-Insert-Business-Rule.png" width="200px">
    </kbd>
<br>

2. Ensure you are working in the xMatters application scope. Click on the __"here"__ link to edit the record if you are not.
    <kbd>
      <img src="/media/xMatters-Scope.png" width="550px">
    </kbd>
<br><br>

3. On __"When to run"__, Add the Following Filter Condition:<br>

    _"Major incident state"_    __is__   _"Accepted"_

    <kbd>
      <img src="/media/Insident-Insert-BR-Change.png" width="550px">
    </kbd>

4. Click Update.



## You can import the script changes using XML files if ALL of the following are true: 
  - You __HAVE__ ServiceNow xMatters integration __v5.0__.<br>
  - You have __NOT__ made any modifications to the script include __xMattersConfig__.<br>
  - You have __NOT__ made any modifications to the script include __xMattersIncident__.<br>

If any of the above conditions are __NOT__ true, __DO NOT Import XML Files__, follow instructions [here](#manually-update-servicenow-script-includes) instead.


### Import the following XML files

1. Import the [xMattersConfig Script Include XML File](ServiceNowUpdates/xMattersConfig-script_include.xml)

    Follow the ServiceNow instructions [here](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/development-best-practices/task/t_ImportARecordAsXMLData.html).


2. Import the [xMattersIncident Script Include XML File](ServiceNowUpdates/xMattersIncident-script_include.xml)

   Follow the ServiceNow instructions [here](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/development-best-practices/task/t_ImportARecordAsXMLData.html).

3. The installation is complete, now you can [Configuring ServiceNow Major Incident Process](#configuring-servicenow-major-incident-process).


To import you can right click in the header of any list (Users, Groups, Incidents, etc). Instructions above include additional permissions that may be required.

<kbd>
  <img src="/media/SNOW-Import-xml.png" width="400px">
</kbd>
<br><br>

## Manually Update ServiceNow Script Includes

### Follow This Step if ANY of the following are true: 
- If you have installed any version of xMatters ServiceNow integration other than v5.0.<br>
- Servicenow __xMattersConfig__ script include __HAS__ been modified.<br>
- ServiceNow __xMattersIncident__ Script include __HAS__ been modified.<br>

_If your version of xMatters ServiceNow integration is not v5.0 you can [upgrade](https://support.xmatters.com/hc/en-us/articles/115004327803-ServiceNow-integration-version-5-0-) to v5.0. Alternatively, you can still use this enhancement but will need to make highlighted changes keeping in mind that your code may differ slightly.  You should be able to use this same method with older versions with little or no modifications. The surrounding code may differ slightly._


### Modify _xMattersConfig_ script include

1. Search for __"xMattersConfig"__ and open the Script include.

    <kbd>
      <img src="/media/xMattersConfig-Search.png" width="200px">
    </kbd>
<br>

2. Ensure you are working in the xMatters application scope. Click on the __"here"__ link to edit the record if you are not.
    <kbd>
      <img src="/media/xMatters-Scope.png" width="550px">
    </kbd>
<br><br>

3. Locate and update the __TRIGGER_RULES: {}__ object. This should be around line 39.


  __Original Code:__
  ```js
  TRIGGER_RULES: {
    ASSIGNMENT: 'Assignment',
    PRIORITY_UPGRADE: 'Priority Upgrade',
    SLA_ALERT: 'SLA Alert',
    REOPENED: 'Reopened',
    DELETE: 'Delete'
  },
  ```
<br><br>
__Make the following code changes, highlighted with the comments:__ <br>
_//***********************************************************_<br>
_//[xmLabs - Major Incident Management Enhancement - xMatters]_<br>
_//***********************************************************_<br>
    and <br>
_//***********************************************************_<br>
_//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]_<br>
_//***********************************************************_<br>

  __Updated Code:__
  ```js
  TRIGGER_RULES: {

//***********************************************************
//[xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************

    MAJOR_INCIDENT_ACCEPTED: 'Major Incident Confirmed',

//***********************************************************
//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************

    ASSIGNMENT: 'Assignment',
    PRIORITY_UPGRADE: 'Priority Upgrade',
    SLA_ALERT: 'SLA Alert',
    REOPENED: 'Reopened',
    DELETE: 'Delete'
  },
  ```

4. Save the script.
    <kbd>
      <img src="/media/Save.png">
    </kbd>





### Modify _xMattersIncident_ script include

1. Search for "xMattersIncident" and open the Script include.

    <kbd>
      <img src="/media/xMattersIncident-Search.png" width="200px">
    </kbd>
<br>

2. Ensure you are working in the xMatters application scope. Click on the __"here"__  link to edit the record if you are not.
    <kbd>
      <img src="/media/xMatters-Scope.png" width="550px">
    </kbd>
<br><br>

3. Make the following modifications:

<br><br>

  A. Locate and update the __isUpdateValid__ function . This should be around line 90.

  __Original Code:__
  ```js
  /**
    * Condition for the "Update" Incident Business Rule
    * @param  {GlideRecord[Incident]}  incident  the "current" incident glide record
    * @param  {GlideRecord[Incident]}  incident_prev  the "previous" incident glide record
    * @return {Boolean}          Whether or not the Business Rule should fire
  */
  isUpdateValid: function (incident, incident_prev) {
      var shouldRun = false;
      if (!this.config.INCIDENT.ENABLED) {
          reason = 'feature is disabled';
      } else {
          var criteria = this.getUpdateCriteria(incident, incident_prev);
          reason = 'no incident condition was met';
          if (criteria.isValidPriority) {
              if (criteria.isActive) {
        
          if (criteria.isPriorityUpgrade || !criteria.wasActive || criteria.isSLAEscalation || criteria.isAssignment || !criteria.wasAcceptedMajorIncident) {
            
            shouldRun = true;
            reason = '| CONDITION : met active |';
          }					
              } else if (criteria.wasActive) {
                  shouldRun = true;
                  reason = '| CONDITION : Closure |';
              }
          } else if (criteria.wasValidPriority) {
              shouldRun = true;
              reason = '| CONDITION : Priority Downgrade |';
          } else {
              reason = 'Priority not in configured priority | ' + incident.priority + ' !IN ' +
                  this.config.INCIDENT.VALID_PRIORITIES;
          }
      }
      if (!shouldRun) {
          this.log.debug('isUpdateValid: Bypassing Incident Business Rule | Reason: ' + reason);
      }
      return shouldRun;
  },
  ```


<br><br>
__Make the following code changes, highlighted with the comments:__ <br>
_//***********************************************************_<br>
_//[xmLabs - Major Incident Management Enhancement - xMatters]_<br>
_//***********************************************************_<br>
    and <br>
_//***********************************************************_<br>
_//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]_<br>
_//***********************************************************_<br>

  __Updated Code:__
  ```js
  /**
    * Condition for the "Update" Incident Business Rule
    * @param  {GlideRecord[Incident]}  incident  the "current" incident glide record
    * @param  {GlideRecord[Incident]}  incident_prev  the "previous" incident glide record
    * @return {Boolean}          Whether or not the Business Rule should fire
  */
  isUpdateValid: function (incident, incident_prev) {
      var shouldRun = false;
      if (!this.config.INCIDENT.ENABLED) {
          reason = 'feature is disabled';
      } else {
          var criteria = this.getUpdateCriteria(incident, incident_prev);
          reason = 'no incident condition was met';
          if (criteria.isValidPriority) {
              if (criteria.isActive) {



//***********************************************************
//[xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************

        if (criteria.isAcceptedMajorIncident) {
          if (criteria.isPriorityUpgrade || !criteria.wasActive ||
            criteria.isSLAEscalation || criteria.isAssignment || !criteria.wasAcceptedMajorIncident || criteria.isSnowAssignment) {
            
            shouldRun = true;
            reason = '| CONDITION : met active |';
          }
        }	

//***********************************************************				
//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************



              } else if (criteria.wasActive) {
                  shouldRun = true;
                  reason = '| CONDITION : Closure |';
              }
          } else if (criteria.wasValidPriority) {
              shouldRun = true;
              reason = '| CONDITION : Priority Downgrade |';
          } else {
              reason = 'Priority not in configured priority | ' + incident.priority + ' !IN ' +
                  this.config.INCIDENT.VALID_PRIORITIES;
          }
      }
      if (!shouldRun) {
          this.log.debug('isUpdateValid: Bypassing Incident Business Rule | Reason: ' + reason);
      }
      return shouldRun;
  },
  ```

<br><br>

  B. Locate and update the __beforeUpdate__ function . This should be around line 150.


  __Original Code:__
  ```js
  /**
    * Logic for the "Update" Incident Business Rule - Will conditionally create an incident event
    * @param  {GlideRecord[Incident]}  incident  the "current" incident glide record
    * @param  {GlideRecord[Incident]}  incident_prev  the "previous" incident glide record
  */
  beforeUpdate: function (incident, incident_prev) {
      var triggerRule = '';
      var eventParameters;
      var criteria = this.getUpdateCriteria(incident, incident_prev);

      if (criteria.isValidPriority) {
          if (criteria.isActive) {
            
        if (criteria.isPriorityUpgrade) { // Priority escalation
          triggerRule = this.config.INCIDENT.TRIGGER_RULES.PRIORITY_UPGRADE;
        } else if (!criteria.wasActive) { // Reopen
          triggerRule = this.config.INCIDENT.TRIGGER_RULES.REOPENED;
        } else if (criteria.isSLAEscalation) { // SLA Alert
          triggerRule = this.config.INCIDENT.TRIGGER_RULES.SLA_ALERT;
        } else if (criteria.isAssignment) { // assignee/group flow
          triggerRule = this.config.INCIDENT.TRIGGER_RULES.ASSIGNMENT;
        } 
                      
        if (triggerRule !== '') {
          eventParameters = {
            triggerRule: triggerRule,
            terminate: true,
            sendEvent: true
          };
        }
      
        } else if (criteria.wasActive) { // Issue resolved
            eventParameters = {
                triggerRule: this.config.INCIDENT.TRIGGER_RULES.DELETE,
                terminate: true,
                sendEvent: false
            };
    
        }
      } else if (criteria.wasValidPriority) { // Priority decreased
          eventParameters = {
              triggerRule: this.config.INCIDENT.TRIGGER_RULES.DELETE,
              terminate: true,
              sendEvent: false
          };

      }
      if (typeof eventParameters !== 'undefined' && eventParameters !== null) {
          eventParameters.userName = gs.getUserName();
          this.log.debug("beforeUpdate: Queuing event from incident update business rule, eventParameters: " + this.json.encode(eventParameters));

          if (!incident) {
              this.log.debug('beforeUpdate: "incident" parameter not set or null; using "current": ' + this.json.encode(current));
              incident = current;
          }
          gs.eventQueue(this.getEventQueue(incident.number), incident, this.json.encode(eventParameters), 'incidentUpdate');
      }
  },
  ```


<br><br>
__Make the following code changes, highlighted with the comments:__ <br>
_//***********************************************************_<br>
_//[xmLabs - Major Incident Management Enhancement - xMatters]_<br>
_//***********************************************************_<br>
    and <br>
_//***********************************************************_<br>
_//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]_<br>
_//***********************************************************_<br>

  __Updated Code:__
  ```js
  /**
    * Logic for the "Update" Incident Business Rule - Will conditionally create an incident event
    * @param  {GlideRecord[Incident]}  incident  the "current" incident glide record
    * @param  {GlideRecord[Incident]}  incident_prev  the "previous" incident glide record
  */
  beforeUpdate: function (incident, incident_prev) {
      var triggerRule = '';
      var eventParameters;
      var criteria = this.getUpdateCriteria(incident, incident_prev);

      if (criteria.isValidPriority) {
          if (criteria.isActive) {
      
//***********************************************************
//[xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************

      if (criteria.isAcceptedMajorIncident) {

//***********************************************************
//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************


        if (criteria.isPriorityUpgrade) { // Priority escalation
          triggerRule = this.config.INCIDENT.TRIGGER_RULES.PRIORITY_UPGRADE;
        } else if (!criteria.wasActive) { // Reopen
          triggerRule = this.config.INCIDENT.TRIGGER_RULES.REOPENED;
        } else if (criteria.isSLAEscalation) { // SLA Alert
          triggerRule = this.config.INCIDENT.TRIGGER_RULES.SLA_ALERT;
        } else if (criteria.isAssignment) { // assignee/group flow
          triggerRule = this.config.INCIDENT.TRIGGER_RULES.ASSIGNMENT;
        } 


//***********************************************************
//[xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************

        else if (criteria.wasAcceptedMajorIncident == false && criteria.isAcceptedMajorIncident == true) { // Major Incident confirmed
              triggerRule = this.config.INCIDENT.TRIGGER_RULES.MAJOR_INCIDENT_ACCEPTED;
          }

//***********************************************************
//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************


        if (triggerRule !== '') {
          eventParameters = {
            triggerRule: triggerRule,
            terminate: true,
            sendEvent: true
          };
        }
        
//*********************************************************** 
//[xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************

      }

//***********************************************************
//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************
      
          } else if (criteria.wasActive) { // Issue resolved
              eventParameters = {
                  triggerRule: this.config.INCIDENT.TRIGGER_RULES.DELETE,
                  terminate: true,
                  sendEvent: false
              };
      
          }
      } else if (criteria.wasValidPriority) { // Priority decreased
          eventParameters = {
              triggerRule: this.config.INCIDENT.TRIGGER_RULES.DELETE,
              terminate: true,
              sendEvent: false
          };

      }
      if (typeof eventParameters !== 'undefined' && eventParameters !== null) {
          eventParameters.userName = gs.getUserName();
          this.log.debug("beforeUpdate: Queuing event from incident update business rule, eventParameters: " + this.json.encode(eventParameters));

          if (!incident) {
              this.log.debug('beforeUpdate: "incident" parameter not set or null; using "current": ' + this.json.encode(current));
              incident = current;
          }
          gs.eventQueue(this.getEventQueue(incident.number), incident, this.json.encode(eventParameters), 'incidentUpdate');
      }
  },
  ```

<br><br>

  C. Locate and update the __getUpdateCriteria__ function . This should be around line 150.

  __Original Code:__
  ```js
  /**
    * Gets a view of the state of the current record based on the different business rule
    * criteria.
    * @param  {GlideRecord[Incident]}  incident  the "current" incident glide record
    * @param  {GlideRecord[Incident]}  incident_prev  the "previous" incident glide record
    * @return {Object}          Returns an object with information about the state of the current incident
    *                                and what type of conditions/criteria are being met
  */
  getUpdateCriteria: function (incident, incident_prev) {
      var criteria = {
          isValidPriority: this.isIncidentPriorityValid(incident),
          wasValidPriority: this.isIncidentPriorityValid(incident_prev),
          isActive: this.isIncidentActive(incident),
          wasActive: this.isIncidentActive(incident_prev),
      };
      if (criteria.isValidPriority && criteria.isActive) {
          criteria.isPriorityUpgrade = incident.priority < incident_prev.priority;
          criteria.isSLAEscalation = incident.escalation.changes() && incident.escalation > incident_prev.escalation &&
              incident_prev.escalation != -1;
    criteria.isAssignment = (incident.assigned_to.changes() && !incident.assigned_to.nil()) ||
              (!incident.assignment_group.nil() && (incident.assigned_to.changes() || incident.assignment_group.changes()));
      
      }

      this.log.debug('getUpdateCriteria: Criteria: ' + global.JSON.stringify(criteria));
      return criteria;
  },
  ```


<br><br>
__Make the following code changes, highlighted with the comments:__ <br>
_//***********************************************************_<br>
_//[xmLabs - Major Incident Management Enhancement - xMatters]_<br>
_//***********************************************************_<br>
    and <br>
_//***********************************************************_<br>
_//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]_<br>
_//***********************************************************_<br>

  __Updated Code:__
  ```js
  /**
    * Gets a view of the state of the current record based on the different business rule
    * criteria.
    * @param  {GlideRecord[Incident]}  incident  the "current" incident glide record
    * @param  {GlideRecord[Incident]}  incident_prev  the "previous" incident glide record
    * @return {Object}          Returns an object with information about the state of the current incident
    *                                and what type of conditions/criteria are being met
  */
  getUpdateCriteria: function (incident, incident_prev) {
      var criteria = {


//***********************************************************
//[xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************

    isAcceptedMajorIncident: this.isAcceptedMajorIncident(incident),
    wasAcceptedMajorIncident: this.isAcceptedMajorIncident(incident_prev),

//***********************************************************
//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]
//***********************************************************


          isValidPriority: this.isIncidentPriorityValid(incident),
          wasValidPriority: this.isIncidentPriorityValid(incident_prev),
          isActive: this.isIncidentActive(incident),
          wasActive: this.isIncidentActive(incident_prev),
      };
      if (criteria.isValidPriority && criteria.isActive) {
          criteria.isPriorityUpgrade = incident.priority < incident_prev.priority;
          criteria.isSLAEscalation = incident.escalation.changes() && incident.escalation > incident_prev.escalation &&
              incident_prev.escalation != -1;
    criteria.isAssignment = (incident.assigned_to.changes() && !incident.assigned_to.nil()) ||
              (!incident.assignment_group.nil() && (incident.assigned_to.changes() || incident.assignment_group.changes()));
      
      }

      this.log.debug('getUpdateCriteria: Criteria: ' + global.JSON.stringify(criteria));
      return criteria;
  },
  ```

<br><br>

  D. Directly after the __getUpdateCriteria__ function edited above, add the following code:

  ```js
//[xmLabs - Major Incident Management Enhancement - xMatters]

  isAcceptedMajorIncident : function(incidentRec){
    return (incidentRec.major_incident_state == 'accepted');
  },

//End Changes [xmLabs - Major Incident Management Enhancement - xMatters]
  ```

<br>

  E. Save the script.

  <kbd>
    <img src="/media/Save.png">
  </kbd>


<br><br>


# Configuring ServiceNow Major Incident Process

__To make this work, you will either need to:__

- [Create Trigger Rules for Major Incident in ServiceNow](https://docs.servicenow.com/bundle/kingston-it-service-management/page/product/incident-management/task/major-incident-trigger-rules.html)
<font color="red">This method will propose or approve incidents as major as soon as they are created based on certain conditions that you set.</font>


  _OR_

- [Proposing an incident as a major incident candidate in ServiceNow](https://docs.servicenow.com/bundle/kingston-it-service-management/page/product/incident-management/task/propose-incident-as-candidate.html)
This can be done from the incident ticket Additional Actions hamburger menu.

__Once you have Proposes a Major Incident Candidate in ServiceNow you can:__
<br>
- [Accept or Reject a major incident candidate](https://docs.servicenow.com/bundle/kingston-it-service-management/page/product/incident-management/task/accept-reject-major-incident-candidate.html)

__You can also use the following to manage Major Incidents in ServiceNow:__

- [Major Incident Workbench](https://docs.servicenow.com/bundle/kingston-it-service-management/page/product/incident-management/concept/major-incident-workbench.html)
- [Major Incident Dashboard](https://docs.servicenow.com/bundle/kingston-it-service-management/page/product/incident-management/concept/major-incident-overview.html)


# Troubleshooting

If the events are not making it into xMatters, then check out the System Logs in ServiceNow for any error messages. This enhancement uses the same logging setting as the Incident workflow, so update the Logging setting in the main xMatters Configuration page to get more or less detail.