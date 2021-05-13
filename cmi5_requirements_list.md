## Requirements:

### 3.0

* IRI’s in this spec MUST be fully qualified and not IRI References.

### 4.1

* An Assignable Unit MUST conform to all requirements as specified in the xAPI specification.

### 4.2

* The LMS MUST conform to all LRS requirements as specified in the xAPI specification.
* The LMS MUST have an account which is able to retrieve all Resource data (from the Statement API, etc, including attachments and extensions) about another distinct user across multiple sessions for that user.

### 4.4

* A course MUST be bundled with course structure data that conform to all requirements listed in Section 13 and Section 14.
* Course structure data MUST NOT implement any features or functionality (optional or mandatory) described in this specification in a non-conforming manner.

### 6.0

* The LMS MUST implement an LRS as defined in the xAPI specification.
* The LMS MUST implement additional "State API" requirements to initialize the AU state as defined in Section 10.
* The LMS MUST implement the runtime launch interface as defined in Section 8.0.
* The LMS MUST implement additional xAPI "Statement API" requirements as defined in Section 9.
* The LMS MUST implement additional xAPI "Agent Profile API" requirements as defined in Section 11.
* The LMS MUST implement course handling as defined in Section 6.1.

### 6.1

* The LMS MUST implement the import of the Course Structure defined in Section 13 and the Course Package defined in Section 14.
* The LMS MUST support course structures containing more than 1000 AUs.
* The LMS MUST support course structures conforming to the XSD schema defined in Section 13.2.

### 6.2

* The LMS MUST implement the State API Requirements as defined in Section 10.

### 6.3

* The LMS MUST NOT provide permissions/credentials which allow the AU to issue voiding Statements.
* The LMS MUST Void statements that are NOT rejected AND conflict with the "Statement API" requirements as defined in Section 9.

### 7.0

* An AU MUST implement the runtime launch interface as defined in Section 8.
* An AU MUST implement runtime communication as defined in the xAPI Specification.
* An AU MUST implement State API requirements in this specification as defined in Section 10.
* An AU MUST implement Profile API requirements in this specification as defined in Section 11.
* An AU MUST implement Statement API requirements as defined in Section 7.1.

### 7.1

* The AU MUST issue a statement to the LRS after being launched, initialized, and ready for learner interaction using the Initialized verb as described in Section 9.3.2.
* The AU MUST issue a Terminated statement to the LRS as described in Section 9.3.8 as the last statement in a session.
* "cmi5 allowed" statements posted by the AU MUST occur between cmi5 statements using the "Initialized" verb and the "Terminated" verb.

### 8.1

* The AU MUST be launched by the LMS using one of the following methods [OwnWindow or AnyWindow], depending on the launchMethod in the Course Structure.
* When the launchMethod is "OwnWindow", the LMS MUST use one of the following - Spawning a new browser window for the AU or Re-directing the existing browser window to the AU.
* When the launchMethod is "AnyWindow", the LMS MUST choose the window context of the AU.
* Regardless of the launchMethod the AU MUST be launched by the LMS with a URL having query string launch parameters as defined in Section 8.1.
* The launch parameters MUST be name/value pairs in a query string appended to the URL that launches the AU.
* If the AU's URL requires a query string for other purposes, then the names MUST NOT collide with named parameters defined below.
* The AU MUST have the ability to process the query string launch parameters in any order.
* Each value for the associated names MUST be URL-encoded.
* The LMS MUST place the "endpoint" in the query string.
* The AU MUST get the "endpoint" value from the query string.
* The AU MUST use the "endpoint" value as the Base Endpoint for xAPI requests.
* The LMS MUST place the "fetch" in the Launch URL.
* The authorization token returned by the "fetch" URL MUST be limited to the duration of a specific user session.
* The AU MUST get the "fetch" value from the query string.
* The AU MUST make an HTTP POST to the "fetch" URL to retrieve the authorization token as defined in section 8.2.
* The AU MUST place the authorization token in the Authorization headers of all HTTP requests made to the endpoint using the xAPI.
* The LMS MUST populate the "actor" parameter in the query string based on the authenticated learner's identity conforming to Section 9.2
* The AU MUST get the "actor" value from the query string.
* The AU MUST use the "actor" value in xAPI requests that require an "actor" property.
* The LMS MUST place the value for "registration" in the query string based on the authenticated learner's corresponding enrollment for the Course that the AU being launched is a member of.
* The AU MUST get the "registration" value from the query string.
* The AU MUST use the "registration" value in xAPI requests that require a "registration".
* The LMS MUST generate a unique activityId for the AU.
* The LMS MUST place its value in the query string.
* The activityId generated MUST NOT match the AU's id (publisher id) from the course structure.
* The LMS MUST use the same generated activityId on all subsequent launches (for the same AU) within the same registration.
* The AU MUST get the "activityId" value from the query string.
* The AU MUST use the "activityId" value as the id property of the Object in all "cmi5 defined" statements.

### 8.2

* The LMS MUST include the "fetch" name/value pair in the launch URL.
* The AU MUST make an HTTP POST to the "fetch" URL to retrieve an authorization token.
* The "fetch" URL MUST return a JSON structure using a Content-Type of "application/json".
* The structure MUST be an object with the property "auth-token" in the first successful response.
* The AU MUST place the "auth-token" in the HTTP header, as defined in <a href='http://tools.ietf.org/html/rfc1945#section-11'>RFC 1945 - 11.1 Basic Authentication Scheme</a>, in all subsequent xAPI communications with the LMS.
* The authorization token returned by the "fetch" URL MUST be limited to the duration of the session.
* The LMS MUST place the value for "auth-token" in a JSON structure, as shown in Section 8.2.1, in its response to a "fetch" URL request.
* The response MUST have a Content-Type of "application/json".
* The HTTP status code MUST be "200" for a valid request (including one that generates an error as described in section 8.2.3).
* The AU MUST get the "auth-token" value using an HTTP POST to the "fetch" URL.
* The AU MUST place the authorization token in the Authorization headers of all HTTP requests made to the endpoint using the xAPI.
* The "fetch" URL is a "one-time use" URL and MUST NOT return an "auth-token" more than once.

### 9.1

* The AU MUST assign a statement id property in UUID format (as defined in the xAPI specification) for all statements it issues.

### 9.2

* The Actor property MUST be defined by the LMS.
* The Actor property for all "cmi5 defined" statements MUST be of objectType "Agent".
* The Actor property MUST contain an "account" IFI as defined in the xAPI specification.

### 9.3

* AUs MUST use the below verbs that are indicated as mandatory in other sections of this specification.
* Verbs MUST NOT be duplicated (in cmi5 defined statements).
* More than one of the set of {"Passed","Failed"} verbs MUST NOT be used (in cmi5 defined statements).
* The "Initialized" verb MUST be the first statement (cmi5 allowed or defined).
* The "Terminated" verb MUST be the last statement (cmi5 allowed or defined).
* Exactly zero or one "Completed" cmi5 defined statement MUST be used per registration.
* Exactly zero or one "Passed" cmi5 defined statement MUST be used per registration.
* A "Failed" statement must not follow a "Passed" statement (in cmi5 defined statements) per registration.
* The LMS MUST record and provide reporting for all statements regardless of the verbs used in statements sent by AUs.
* LMS MUST NOT issue more than one abandoned statement for a session.
* LMS MUST NOT issue more than one waived statement per session and MUST not issue more than one waived statement per registration per AU.
* The LMS MUST use this verb, "Launched", in a statement recorded in the LRS before launching an AU.
* The LMS MUST NOT issue multiple statements with "Launched" for the same AU within a given AU session.
* The "Initialized" statement MUST follow, within a reasonable period of time, the "Launched" statement created by the LMS.
* The AU MUST use "Initialized" in the first statement (of any kind) in the AU session.
* The AU MUST NOT issue multiple statements with "Initialized" for the same AU within a given AU session.
* The AU MUST send a statement containing the "Completed" verb when the learner has experienced all relevant material in an AU.
* The AU MUST NOT issue multiple statements with "Completed" for the same AU within a given course registration for a given learner.
* The LMS MUST use "Completed" statements based on the "moveOn" criteria for the AU as provided in the LMS Launch Data.
* The AU MUST send a statement containing the "Passed" verb when the learner has attempted and passed the AU.
* If the "Passed" statement contains a (scaled) score, the (scaled) score MUST be equal to or greater than the "masteryScore" indicated in the LMS Launch Data.
* The AU MUST NOT issue multiple statements with "Passed" for the same AU within a given course registration for a given learner.
* The LMS MUST use "Passed" statements based on the "moveOn" criteria for the AU as provided in the LMS Launch Data.
* The AU MUST send a statement containing the "Failed" verb when the learner has attempted and failed the AU.
* If the "Failed" statement contains a (scaled) score, the (scaled) score MUST be less than the "masteryScore" indicated in the LMS Launch Data.
* In the absence of a "Terminated" statement, the LMS MUST make the determination if an AU abnormally terminated a session by monitoring new statement or state API requests made for the same learner/course registration for a different AU.
* The LMS MUST record an "Abandoned" statement on behalf of the AU indicating an abnormal session termination.
* The LMS MUST NOT allow any statements to be recorded for a session after recording an "Abandoned" statement.
* The LMS MUST use this verb, "Waived", in a statement recorded in the LRS when it determines that the AU may be waived.
* A statement containing a "Waived" verb MUST include a "reason" in the extension property of the Statement Result.
* The LMS MUST generate a unique session id for the statement containing a "Waived" verb and MUST NOT issue any other statements (except for statements with the "Satisfied" verb) using that session id.
* The LMS MUST NOT issue multiple statements with "Waived" for the same AU within a course registration.
* The AU MUST send a statement containing the "Terminated" verb.
* This statement, "Terminated", MUST be the last statement (of any kind) sent by the AU in a session.
* The LMS MUST use the "Terminated" statement to determine that the AU session has ended.
* Upon receiving a "Terminated" statement, the LMS MUST wait a specified period of time (defined by the LMS implementation) after which it MUST reject statements (of any kind) for the AU session.
* The LMS MUST use the "Satisfied" statement when the learner has met the moveOn criteria of all AU's in a block.
* In this statement, "Satisfied", the LMS MUST use the LMS generated block id as the Object id and use "https://w3id.org/xapi/cmi5/activitytype/block" as the value of the "type" property in the Object's Definition.
* The LMS MUST generate a unique block id for the Satisfied Statement.
* The generated Block id MUST NOT match the publisher’s ID from the course structure.
* The LMS MUST also use the "Satisfied" statement when the learner has met the moveOn criteria for all AU's in a course.
* In this statement, "Satisfied" the LMS MUST use the LMS generated course id as the Object id and use "https://w3id.org/xapi/cmi5/activitytype/course" as the value of the "type" property in the Object's Definition.
* The LMS MUST generate a unique course id for the Satisfied Statement.
* The generated course id MUST NOT match the publisher’s ID from the course structure.
* For all "Satisfied" statements triggered as a result of an AU launch session, the LMS MUST use the session id from the AU launch in the statements.

### 9.4

* An Object MUST be present, as specified in this section, in all "cmi5 defined" statements.
* When the Object is the AU, the value of the Object's "id" property for a given AU MUST match the activityId defined in the launch URL.

### 9.5

* If a score is reported by an AU, the verb MUST be consistent with "masteryScore" (if defined for the AU in the LMS Launch Data).
* cmi5 defined statements, other than "Passed" or "Failed", MUST NOT include the "score" property.
* The AU MUST provide the "min" and "max" values for "score" when the "raw" value is provided.
* The "success" property of the result MUST be set to true for the following cmi5 defined statements ["Passed", "Waived"].
* The "success" property of the result MUST be set to false for the following cmi5 defined statements ["Failed"].
* cmi5 defined statements, other than "Passed", "Waived" or "Failed", MUST NOT include the "success" property.
* The "completion" property of the result MUST be set to true for the following cmi5 defined statements ["Completed", "Waived"].
* cmi5 defined statements, other than "Completed" or "Waived", MUST NOT include the "completion" property.
* The AU MUST include the "duration" property in "Terminated" statements.
* The AU MUST include the "duration" property in "Completed" statements.
* The AU MUST include the "duration" property in "Passed" statements.
* The AU MUST include the "duration" property in "Failed" statements.
* The duration property MUST be included in "Abandoned" statements.
* In the absence of such methods [LMS specific methods of determination], the duration property MUST be set as the total session time, calculated as the time between the "Launched" statement and the last statement (of any kind) issued by the AU.
* The LMS MUST set this value [reason extension] in statements it makes with the verb "Waived".

### 9.6
* All cmi5 defined statements MUST contain a context that includes all properties as defined in this section [9.6].
* The value for the registration property used in the context object MUST be the value provided by the LMS.
* The LMS MUST generate this value and pass it to the AU via the launch URL.
* The LMS MUST evaluate MoveOn criteria in the course structure at the time of registration.
* All cmi5 defined statements must include all properties and values defined in the the contextActivites of the context template.
* An Activity object with an "id" of "https://w3id.org/xapi/cmi5/context/categories/cmi5" in the "category" context activities list MUST be used in cmi5 defined statements as described in section 7.1.3.
* cmi5 defined statements with a Result object (Section 9.5) that include either "success" or "completion" properties MUST have an Activity object with an "id" of "https://w3id.org/xapi/cmi5/context/categories/moveon" in the "category" context activities list.
* Other statements [than those with a `result.completion` or `result.success` property] MUST NOT include this Activity [the moveOn Category Activity].
* The LMS MUST include an Activity object with an "id" property whose value is the unaltered value of the AU's "id" attribute from the course structure in the "grouping" context activities list in the "contextTemplate" as described in the State API prior to launching an AU.
* The LMS MUST include the publisher id Activity in the "grouping" context activities list for all "cmi5 defined" and "cmi5 allowed" statements it makes directly in the LRS.
* Statements MUST NOT include extensions that conflict with or duplicate the ones specified in Section 9.6.3.
* The value for session ID MUST be generated by the LMS.
* The LMS MUST record the session ID in the State API prior to launching an AU.
* The LMS MUST provide the session ID in the context as an extension for all "cmi5 defined" and "cmi5 allowed" statements it makes directly in the LRS.
* The LMS MUST generate a unique session id and use it in all Satisfied statements triggered outside of an AU launch session.
* An AU MUST include the session ID provided by the LMS in the context as an extension for all "cmi5 defined" and "cmi5 allowed" statements it makes directly in the LRS.
* The LMS MUST add masteryScore to the context of a "Launched" statement when it is provided in the LMS launch data.
* An AU MUST include the "masteryScore" value provided by the LMS in the context as an extension for "passed"/"failed" Statements it makes based on the "masteryScore".
* The LMS MUST add launchMode to the context of a "Launched" statement.
* The LMS MUST put a fully qualified URL equivalent to the one that the LMS used to launch the AU without the name/value pairs included as defined in section 8.1 in the context extensions of the "Launched" statement.
* The LMS MUST add moveOn to the context of a "Launched" statement.
* The LMS MUST add launchParameters to the context of a "Launched" statement when it is provided in the LMS launch data.
* All statements MUST include a timestamp property per the xAPI specification to ensure statement ordering requirements are met.
* All timestamps MUST be recorded in UTC time.

### 10.0

* Prior to launching an AU, the LMS MUST create or update a document in the State API record in the LRS.
* This [the LMS.LaunchData] MUST be a JSON document, as defined in Section 10.0.
* This [activityId property] MUST match the activity id used by the LMS at AU launch time.
* This [agent State API PUT Property] MUST match the actor property generated by the LMS at AU launch time.
* This [registration State API PUT Property] MUST match the registration used by the LMS at AU launch time.
* The LMS MUST include a "contextTemplate" object [in the State API document].
* [The contextTemplate object MUST include] The value for session id placed in an "extensions" property with the id as defined in Section 9.6.3.1.
* [The contextTemplate object MUST include] The publisher id Activity as defined in Section 9.6.2.3 in the "contextActivities.grouping" list
* The AU MUST get the "contextTemplate" value from the "LMS.LaunchData" State document.
* The AU MUST NOT modify or delete the "LMS.LaunchData" State document.
* The AU MUST use the contextTemplate as a template for the "context" property in all xAPI statements it sends to the LMS.
* While the AU may include additional values in the Context object of such statements, it MUST NOT overwrite any values provided in the contextTemplate.
* Normal [launchMode] Indicates to the AU that completion-related data MUST be recorded in the LMS using xAPI statements.
* Browse [launchMode] Indicates to the AU that completion-related data MUST NOT be recorded in the LMS using xAPI statements.
* Review [launchMode] Indicates to the AU that completion-related data MUST NOT be recorded in the LMS using xAPI statements.
* The LMS MUST include a value for "launchMode" [in the State API document].
* The AU MUST conform to the following [statement requirements] based on the value of "launchMode".
* The AU MUST send "Initialized" and "Terminated" verb statements [for a Normal launchMode].
* The AU MUST send other cmi5 defined statements per the requirements defined in section 9.3 [for a Normal launchMode].
* The AU MUST send "Initialized" and "Terminated" verb statements [for a Browse launchMode].
* The AU MUST NOT send other cmi5 defined statements [for a Browse launchMode].
* The AU MUST send "Initialized" and "Terminated" verb statements [for a Review launchMode].
* The AU MUST NOT send other cmi5 defined statements [for a Review launchMode].
* The LMS MUST include the "launchParameters" in the State API document if the "launchParameters" were defined by the course designer in the Course Structure.
* The LMS MUST include a "masteryScore" in the State API document if the "masteryScore" was defined by the course designer in the Course Structure.
* If the AU issues "Passed" or "Failed" statements they MUST be based on the "masteryScore" if provided.
* The LMS must provide a "moveOn" value in the state API document.
* The AU MUST attempt to get the "returnURL" value from the "LMS.LaunchData" state document.
* The AU MUST redirect the current browser window or frame to the "returnURL" when the AU is terminated if the "returnURL" is provided.
* The LMS MUST add an "entitlementKey" object to the "LMS.LaunchData" state document if an "entitlementKey" is present in the Course Structure for the AU.
* The LMS MUST obtain this [entitlementKey.courseStructure] from the Course Structure.
* The LMS MUST obtain the value for the [entitlementKey] alternate property from a source as agreed upon with the AU.

### 11.0

* The Agent used in xAPI Agent Profile requests MUST match the actor property generated by the LMS at AU launch time.
* The AU MUST NOT treat the 403 response [to Learner Preferences change requests] as an error condition.
* The AU MUST retrieve the Learner Preferences document from the Agent Profile on startup.
* When reading or writing to the Agent Profile, the document name MUST be "cmi5LearnerPreferences".
* The [Agent Profile] document content MUST be a JSON object with the "languagePreference" and "audioPreference" properties as described in Section 11.1 and 11.2.
* The languagePreference MUST be a comma-separated list of RFC 5646 Language Tags as indicated in the xAPI specification (Section 5.2).
* In the [languagePreference] list, languages MUST be specified in order of user preference.
* The AU MUST turn the audio on or off at startup based on [the audioPreference property] value.

### 13.0
* All leading/trailing whitespace MUST be removed by the LMS on import of the course structure for all of the data elements defined in this section.
* This id, "block" element "id" attribute value, MUST be unique within the course structure.
* This id, "objective" element "id" attribute value, MUST be unique within the course structure.
* This id, "au" element "id" attribute value, MUST be unique within the course structure.
* Regardless of the value of "scheme", the remaining portion of the URL ["au" element "url" attribute] MUST conform to RFC1738 - Uniform Resource Locators (URL).
* If the url includes a query string, the values from that query string MUST be merged with the cmi5 parameters at launch time.
* For that [course designers using their own namespaced elements] they MUST provide a XML Schema Definition.
* These extensions [Vendor Specific Metadata] MUST keep the course structure XML valid.
* All course structures created for LMS import functionality and created by the LMS for export MUST conform to this [https://w3id.org/xapi/profiles/cmi5/v1/CourseStructure.xsd] XSD and be named "cmi5.xml".

### 14.0

* For the course import and export defined in Section 6.1, the LMS MUST support all of the following formats: Zip32, Zip64, course structure XML file.
* The two ZIP file formats MUST follow the specification defined at https://www.pkware.com/support/zip-app-note.
* When the ZIP file is used to package a course, it MUST contain the course structure XML file at its root directory.
* Any media included in a ZIP course package MUST use relative URL references in the Course Structure XML.
* Any media not included in a ZIP course package MUST use fully qualified URL references in the Course Structure XML.
* When a course structure XML file is provided without a ZIP file package, all URL references MUST be fully qualified.
