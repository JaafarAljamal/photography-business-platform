# Use Cases

## Purpose

This document defines the functional interactions between the system and its actors.

Each use case describes how an actor achieves a specific business goal by interacting with the Photography Business Platform.

The use cases provide the functional behavior baseline for software design, implementation, testing, and future maintenance.

---

## Scope

The use cases defined in this document apply to Version 1.0 of the Photography Business Platform.

They describe the interactions performed by guest visitors, clients, the photographer, and the platform while supporting the complete business lifecycle of the photography business.

---

## Actors

The following actors participate in the use cases defined in this document:

### Photographer

The business owner and system administrator responsible for managing the photography business.

### Guest Visitor

A visitor who browses the public website before submitting a booking request.

### Client

A visitor who has submitted a booking request.

Unless otherwise specified, every capability available to a Guest Visitor remains available to a Client.

### External Systems

The following external systems interact with the platform where applicable:

- Email Service

---

## Use Case Groups

The use cases are organized into functional groups that correspond to the business capabilities of the Photography Business Platform.

The use case groups are:

- Portfolio
- Services
- Inquiry
- Booking
- Payment
- Invoice
- Business Administration

---

## Use Case Template

Each use case in this document follows the same structure:

- Goal
- Primary Actor
- Supporting Actors
- Trigger
- Preconditions
- Main Success Scenario
- Alternative Flows
- Postconditions
- Related Functional Requirements
- Related Business Process

---

## UC-01 — Portfolio

This group contains the use cases related to presenting and managing the photographer's portfolio.

### UC-001 — Browse Portfolio

#### Goal

Allow a guest visitor to browse the photographer's published portfolio in order to evaluate the photographer's work before deciding whether to continue interacting with the photography business.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor opens the Portfolio section of the public website.

#### Preconditions

- The public website is available.
- At least one portfolio item has been published.

#### Main Success Scenario

1. Guest Visitor opens the Portfolio section.
2. The System retrieves all published portfolio items.
3. The System displays the available portfolio items.
4. Guest Visitor browses the displayed portfolio.
5. The use case ends.

#### Alternative Flows

**A1 — No Portfolio Items Available**

1. No published portfolio items exist.
2. The system informs the visitor that no portfolio items are currently available.
3. The use case ends.

#### Postconditions

- No business data is modified.
- The guest visitor has viewed the available published portfolio items.

#### Related Functional Requirements

- FR-001 — Browse Portfolio

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-002 — Browse Portfolio Albums

#### Goal

Allow a guest visitor to browse the available portfolio albums in order to explore the photographer's work through organized photography collections.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor opens the Portfolio Albums section of the public website.

#### Preconditions

- The public website is available.
- At least one portfolio album has been published.

#### Main Success Scenario

1. The guest visitor opens the Portfolio Albums section.
2. The System retrieves all published portfolio albums.
3. The System displays the available portfolio albums.
4. The guest visitor browses the displayed portfolio albums.
5. The use case ends.

#### Alternative Flows

**A1 — No Portfolio Albums Available**

1. The System finds no published portfolio albums.
2. The System informs the visitor that no portfolio albums are currently available.
3. The use case ends.

#### Postconditions

- No business data is modified.
- The visitor has viewed the currently published portfolio albums.

#### Related Functional Requirements

- FR-002 — Browse Portfolio Albums

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-003 — Browse Portfolio Categories

#### Goal

Allow a guest visitor to browse the available portfolio categories in order to quickly find photography work that matches their interests.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor opens the Portfolio Categories section of the public website.

#### Preconditions

- The public website is available.
- At least one portfolio category has been published.

#### Main Success Scenario

1. The guest visitor opens the Portfolio Categories section.
2. The System retrieves all published portfolio categories.
3. The System displays the available portfolio categories.
4. The guest visitor browses the displayed portfolio categories.
5. The use case ends.

#### Alternative Flows

**A1 — No Portfolio Categories Available**

1. The System finds no published portfolio categories.
2. The System informs the visitor that no portfolio categories are currently available.
3. The use case ends.

#### Postconditions

- No business data is modified.
- The visitor has viewed the currently published portfolio categories.

#### Related Functional Requirements

- FR-003 — Browse Portfolio Categories

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-004 — View Portfolio Item

#### Goal

Allow a guest visitor to open a published portfolio item in order to examine the photographer's work in greater detail.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor selects a published portfolio item.

#### Preconditions

- The public website is available.
- The selected portfolio item has been published.

#### Main Success Scenario

1. The guest visitor selects a published portfolio item.
2. The System retrieves the selected portfolio item.
3. The System displays the selected portfolio item in full-screen mode.
4. The guest visitor views the portfolio item.
5. The use case ends.

#### Alternative Flows

**A1 — Portfolio Item Is Not Available**

1. The selected portfolio item is unavailable or no longer published.
2. The System informs the visitor that the selected portfolio item is unavailable.
3. The use case ends.

#### Postconditions

- No business data is modified.
- The selected portfolio item has been displayed to the visitor.

#### Related Functional Requirements

- FR-004 — View Portfolio Item

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-005 — Close Portfolio Item

#### Goal

Allow a guest visitor to close the currently displayed portfolio item and return to the portfolio browsing view.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor chooses to close the currently displayed portfolio item.

#### Preconditions

- A published portfolio item is currently displayed in full-screen mode.

#### Main Success Scenario

1. The guest visitor requests to close the displayed portfolio item.
2. The System closes the full-screen view.
3. The System returns the visitor to the portfolio browsing view.
4. The use case ends.

#### Alternative Flows

**None**

#### Postconditions

- No business data is modified.
- The visitor returns to the portfolio browsing view.

#### Related Functional Requirements

- FR-005 — Close Portfolio Item

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-006 — View Portfolio Item Details

#### Goal

Allow a guest visitor to view the descriptive information of a published portfolio item in order to better understand the context of the photographer's work.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor requests to view the details of a published portfolio item.

#### Preconditions

- The public website is available.
- The selected portfolio item has been published.

#### Main Success Scenario

1. The guest visitor requests to view the details of the selected portfolio item.
2. The System retrieves the descriptive information associated with the selected portfolio item.
3. The System displays the available portfolio item details.
4. The guest visitor reviews the displayed information.
5. The use case ends.

#### Alternative Flows

**A1 — No Additional Information Available**

1. The selected portfolio item has no descriptive information.
2. The System informs the guest visitor that no additional information is available for the selected portfolio item.
3. The use case ends.

#### Postconditions

- No business data is modified.
- The guest visitor has viewed the available descriptive information associated with the selected portfolio item.

#### Related Functional Requirements

- FR-006 — View Portfolio Item Details

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-007 — Manage Portfolio

#### Goal

Allow the photographer to create, update, publish, hide, and remove portfolio items in order to maintain an accurate and professional portfolio.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the portfolio management page.

#### Preconditions

- The photographer has successfully authenticated.
- The portfolio management page is accessible.

#### Main Success Scenario

1. The photographer opens the portfolio management page.
2. The System displays the existing portfolio items.
3. The photographer creates a new portfolio item or selects an existing portfolio item.
4. The photographer manages the portfolio information, images, albums, and categories.
5. The photographer saves the portfolio item.
6. The System validates the submitted information.
7. The System stores the portfolio item.
8. The photographer chooses whether the portfolio item should be publicly visible.
9. The System publishes or hides the portfolio item accordingly.
10. The use case ends.

#### Alternative Flows

**A1 — Validation Failure**

1. The submitted portfolio information fails validation.
2. The System displays the validation errors.
3. The photographer corrects the information.
4. The use case resumes from Step 5.

#### Postconditions

- The portfolio item has been successfully created or updated.
- The portfolio reflects the photographer's latest changes.
- The portfolio item's visibility status has been updated when applicable.

#### Related Functional Requirements

- FR-007 — Manage Portfolio

#### Related Business Process

- BP-01 — Portfolio Management

---

### UC-008 — Manage Portfolio Albums

#### Goal

Allow the photographer to create, update, publish, hide, and remove portfolio albums in order to organize portfolio items into meaningful collections.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the portfolio album management page.

#### Preconditions

- The photographer has successfully authenticated.
- The portfolio album management page is accessible.

#### Main Success Scenario

1. The photographer opens the portfolio album management page.
2. The System displays the existing portfolio albums.
3. The photographer creates a new portfolio album or selects an existing portfolio album.
4. The photographer manages the album information and its associated portfolio items.
5. The photographer saves the portfolio album.
6. The System validates the submitted information.
7. The System stores the portfolio album.
8. The photographer chooses whether the portfolio album should be publicly visible.
9. The System publishes or hides the portfolio album accordingly.
10. The use case ends.

#### Alternative Flows

**A1 — Validation Failure**

1. The submitted portfolio album information fails validation.
2. The System displays the validation errors.
3. The photographer corrects the information.
4. The use case resumes from Step 5.

#### Postconditions

- The portfolio album has been successfully created or updated.
- The portfolio album organization reflects the photographer's latest changes.
- The portfolio album visibility status has been updated when applicable.

#### Related Functional Requirements

- FR-008 — Manage Portfolio Albums

#### Related Business Process

- BP-01 — Portfolio Management

---

### UC-009 — Manage Portfolio Categories

#### Goal

Allow the photographer to create, update, publish, hide, and remove portfolio categories in order to classify portfolio items by photography type.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the portfolio category management page.

#### Preconditions

- The photographer has successfully authenticated.
- The portfolio category management page is accessible.

#### Main Success Scenario

1. The photographer opens the portfolio category management page.
2. The System displays the existing portfolio categories.
3. The photographer creates a new portfolio category or selects an existing portfolio category.
4. The photographer manages the category information and its associated portfolio items.
5. The photographer saves the portfolio category.
6. The System validates the submitted information.
7. The System stores the portfolio category.
8. The photographer chooses whether the portfolio category should be publicly visible.
9. The System publishes or hides the portfolio category accordingly.
10. The use case ends.

#### Alternative Flows

**A1 — Validation Failure**

1. The submitted portfolio category information fails validation.
2. The System displays the validation errors.
3. The photographer corrects the information.
4. The use case resumes from Step 5.

#### Postconditions

- The portfolio category has been successfully created or updated.
- The portfolio classification reflects the photographer's latest changes.
- The portfolio category visibility status has been updated when applicable.

#### Related Functional Requirements

- FR-009 — Manage Portfolio Categories

#### Related Business Process

- BP-01 — Portfolio Management

---

## UC-02 — Services

### UC-010 — Browse Photography Services

#### Goal

Allow a guest visitor to browse the available photography services in order to understand the types of photography services offered by the business.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor opens the Photography Services section of the public website.

#### Preconditions

- The public website is available.
- At least one photography service has been published.

#### Main Success Scenario

1. The guest visitor opens the Photography Services section.
2. The System retrieves all published photography services.
3. The System displays the available photography services.
4. The guest visitor browses the displayed photography services.
5. The use case ends.

#### Alternative Flows

**A1 — No Photography Services Available**

1. The System finds no published photography services.
2. The System informs the guest visitor that no photography services are currently available.
3. The use case ends.

#### Postconditions

- No business data is modified.
- The guest visitor has viewed the available photography services.

#### Related Functional Requirements

- FR-011 — Browse Photography Services

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-011 — View Photography Service Details

#### Goal

Allow a guest visitor to view the details of a photography service in order to determine whether it meets their requirements.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor selects a published photography service.

#### Preconditions

- The public website is available.
- The selected photography service has been published.

#### Main Success Scenario

1. The guest visitor selects a photography service.
2. The System retrieves the service details.
3. The System displays the photography service details.
4. The guest visitor reviews the displayed information.
5. The use case ends.

#### Alternative Flows

**A1 — Service Is Not Available**

1. The selected photography service is unavailable or no longer published.
2. The System informs the guest visitor that the selected photography service is unavailable.
3. The use case ends.

#### Postconditions

- No business data is modified.
- The guest visitor has viewed the selected photography service.

#### Related Functional Requirements

- FR-012 — View Photography Service Details

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-012 — Browse Service Packages

#### Goal

Allow a guest visitor to browse the available service packages in order to compare the photography offerings provided by the business.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor opens the Service Packages section of the public website.

#### Preconditions

- The public website is available.
- At least one service package has been published.

#### Main Success Scenario

1. The guest visitor opens the Service Packages section.
2. The System retrieves all published service packages.
3. The System displays the available service packages.
4. The guest visitor browses the displayed service packages.
5. The use case ends.

#### Alternative Flows

**A1 — No Service Packages Available**

1. The System finds no published service packages.
2. The System informs the guest visitor that no service packages are currently available.
3. The use case ends.

#### Postconditions

- No business data is modified.
- The guest visitor has viewed the available service packages.

#### Related Functional Requirements

- FR-013 — Browse Service Packages

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-013 — View Service Package Details

#### Goal

Allow a guest visitor to view the details of a service package in order to understand the services included before requesting a booking.

#### Primary Actor

Guest Visitor

#### Supporting Actors

None

#### Trigger

The guest visitor selects a published service package.

#### Preconditions

- The public website is available.
- The selected service package has been published.

#### Main Success Scenario

1. The guest visitor selects a service package.
2. The System retrieves the package details.
3. The System displays the service package details.
4. The guest visitor reviews the displayed information.
5. The use case ends.

#### Alternative Flows

**A1 — Package Is Not Available**

1. The selected service package is unavailable or no longer published.
2. The System informs the guest visitor that the selected service package is unavailable.
3. The use case ends.

#### Postconditions

- No business data is modified.
- The guest visitor has viewed the selected service package.

#### Related Functional Requirements

- FR-014 — View Service Package Details

#### Related Business Process

- BP-05 — Client Acquisition

---

### UC-014 — Manage Photography Services

#### Goal

Allow the photographer to create, update, publish, hide, and remove photography services in order to maintain an accurate catalog of services offered by the business.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the photography service management page.

#### Preconditions

- The photographer has successfully authenticated.
- The photography service management page is accessible.

#### Main Success Scenario

1. The photographer opens the photography service management page.
2. The System displays the existing photography services.
3. The photographer creates a new service or selects an existing service.
4. The photographer manages the service information.
5. The photographer saves the service.
6. The System validates the submitted information.
7. The System stores the service.
8. The photographer chooses whether the service should be publicly visible.
9. The System publishes or hides the service accordingly.
10. The use case ends.

#### Alternative Flows

**A1 — Validation Failure**

1. The submitted service information fails validation.
2. The System displays the validation errors.
3. The photographer corrects the information.
4. The use case resumes from Step 5.

#### Postconditions

- The photography service has been successfully created or updated.
- The service catalog reflects the photographer's latest changes.
- The service visibility status has been updated when applicable.

#### Related Functional Requirements

- FR-015 — Manage Photography Services

#### Related Business Process

- BP-02 — Service Management

---

### UC-015 — Manage Service Packages

#### Goal

Allow the photographer to create, update, publish, hide, and remove service packages in order to maintain structured commercial photography offerings.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the service package management page.

#### Preconditions

- The photographer has successfully authenticated.
- The service package management page is accessible.

#### Main Success Scenario

1. The photographer opens the service package management page.
2. The System displays the existing service packages.
3. The photographer creates a new package or selects an existing package.
4. The photographer manages the package information and its associated photography services.
5. The photographer saves the package.
6. The System validates the submitted information.
7. The System stores the package.
8. The photographer chooses whether the package should be publicly visible.
9. The System publishes or hides the package accordingly.
10. The use case ends.

#### Alternative Flows

**A1 — Validation Failure**

1. The submitted package information fails validation.
2. The System displays the validation errors.
3. The photographer corrects the information.
4. The use case resumes from Step 5.

#### Postconditions

- The service package has been successfully created or updated.
- The package catalog reflects the photographer's latest changes.
- The package visibility status has been updated when applicable.

#### Related Functional Requirements

- FR-016 — Manage Service Packages

#### Related Business Process

- BP-03 — Package Management

---

## UC-03 — Inquiry

### UC-016 — Submit Inquiry

#### Goal

Allow a guest visitor to submit an inquiry to request information before deciding whether to request a booking.

#### Primary Actor

Guest Visitor

#### Supporting Actors

Email Service

#### Trigger

The guest visitor selects **Contact Us** or **Submit Inquiry**.

#### Preconditions

- The public website is available.
- The inquiry form is accessible.
- The visitor is not required to authenticate.

#### Main Success Scenario

1. The guest visitor opens the inquiry form.
2. The guest visitor enters the required inquiry information.
3. The guest visitor submits the inquiry.
4. The system validates the submitted information.
5. The system records the inquiry.
6. The system triggers the inquiry confirmation process.
7. The use case ends successfully.

#### Alternative Flows

**A1 — Validation fails**

- The system displays the validation errors.
- The guest visitor corrects the information.
- The flow resumes at Step 2.

#### Postconditions

- The inquiry is stored successfully.
- A unique inquiry reference number has been assigned.
- The inquiry becomes available for the photographer.
- An inquiry confirmation process has been initiated.

#### Related Functional Requirements

- FR-017 — Submit Inquiry

#### Related Business Process

- BP-06 — Inquiry Management

---

### UC-017 — Receive Inquiry Confirmation

#### Goal

Notify the guest visitor that the submitted inquiry has been successfully received.

#### Primary Actor

Guest Visitor

#### Supporting Actors

Email Service

#### Trigger

A new inquiry has been successfully recorded.

#### Preconditions

- A valid inquiry exists.
- The inquiry contains a valid email address.

#### Main Success Scenario

1. The system prepares the inquiry confirmation email.
2. The system sends the confirmation email.
3. The guest visitor receives the confirmation email.
4. The use case ends successfully.

#### Alternative Flows

**A1 — Email delivery fails**

- The system records the delivery failure.
- The inquiry remains recorded.
- The system may retry email delivery according to the notification policy.

#### Postconditions

- The inquiry confirmation has been delivered or queued for delivery.
- The inquiry remains recorded regardless of email delivery status.

#### Related Functional Requirements

- FR-018 — Receive Inquiry Confirmation

#### Related Business Process

- BP-06 — Inquiry Management

---

### UC-018 — View Inquiries

#### Goal

Allow the photographer to review submitted inquiries.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the inquiry management page.

#### Preconditions

- The photographer is authenticated.
- At least one inquiry may exist.

#### Main Success Scenario

1. The photographer opens the inquiry list.
2. The system retrieves the recorded inquiries.
3. The system displays the inquiry list.
4. The photographer selects an inquiry.
5. The system displays the inquiry details and conversation history.
6. The use case ends successfully.

#### Alternative Flows

**A1 — No inquiries exist**

- The system displays an empty inquiry list.

#### Postconditions

- The photographer has reviewed one or more inquiries.

#### Related Functional Requirements

- FR-019 — View Inquiries

#### Related Business Process

- BP-06 — Inquiry Management

---

### UC-019 — Respond to Inquiry

#### Goal

Allow the photographer to respond to an inquiry and continue the communication with the visitor.

#### Primary Actor

Photographer

#### Supporting Actors

Email Service

#### Trigger

The photographer decides to reply to an inquiry.

#### Preconditions

- The photographer is authenticated.
- The selected inquiry exists.

#### Main Success Scenario

1. The photographer opens an inquiry.
2. The photographer writes a response.
3. The photographer submits the response.
4. The system records the response.
5. The system sends the response by email.
6. The visitor receives the email.
7. The use case ends successfully.

#### Alternative Flows

**A1 — Email delivery fails**

- The system records the delivery failure.
- The response remains stored.
- The system may retry email delivery.

#### Postconditions

- The response is stored.
- The inquiry conversation history is updated.

#### Related Functional Requirements

- FR-020 — Respond to Inquiry

#### Related Business Process

- BP-06 — Inquiry Management

---

### UC-020 — Manage Inquiries

#### Goal

Allow the photographer to organize and manage inquiries throughout their lifecycle.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer manages inquiry records.

#### Preconditions

- The photographer is authenticated.
- One or more inquiries exist.

#### Main Success Scenario

1. The photographer opens the inquiry management page.
2. The photographer reviews the inquiry status.
3. The photographer updates the inquiry status or organizational information.
4. The system validates the changes.
5. The system stores the updated inquiry information.
6. The use case ends successfully.

#### Alternative Flows

**A1 — Validation fails**

- The system displays the validation errors.
- The photographer corrects the information.
- The flow resumes at Step 3.

#### Postconditions

- Inquiry information is updated successfully.
- Inquiry organization reflects the latest business state.

#### Related Functional Requirements

- FR-021 — Manage Inquiries

#### Related Business Process

- BP-06 — Inquiry Management

---

## UC-04 — Booking

### UC-021 — Submit Booking Request

#### Goal

Allow a client to submit a photography booking request through the system.

#### Primary Actor

Client

#### Supporting Actors

None

#### Trigger

The client decides to request a photography booking.

#### Preconditions

- The client has selected a photography service, package, or intends to request a custom photography service.
- The booking request page is available.

#### Main Success Scenario

1. The client opens the booking request page.
2. The client enters the required booking information.
3. The client submits the booking request.
4. The system validates the submitted information.
5. The system stores the booking request.
6. The system assigns a unique booking reference number.
7. The system generates a unique booking tracking link.
8. The system initiates the booking request confirmation process.
9. The booking request is successfully submitted.

#### Alternative Flows

**A1 — Validation fails**

- The submitted booking information is incomplete or invalid.
- The system displays the validation errors.
- The client corrects the information and resubmits the booking request.

#### Postconditions

- The booking request has been successfully stored.
- A unique booking reference number has been assigned.
- A unique booking tracking link has been generated.
- The booking request is available for photographer review.
- The booking request confirmation process has been initiated.

#### Related Functional Requirements

- FR-022 — Submit Booking Request

#### Related Business Process

- BP-07 — Booking Management

---

### UC-022 — Select Available Booking Date and Time

#### Goal

Allow the client to select a preferred booking date and time from the available booking schedule.

#### Primary Actor

Client

#### Supporting Actors

None

#### Trigger

The client starts creating a booking request.

#### Preconditions

- The client is submitting a booking request.
- The booking request page is available.
- The booking calendar is available.

#### Main Success Scenario

1. The client opens the booking calendar.
2. The system displays the available booking dates and time slots.
3. The client browses the available schedule.
4. The client selects the preferred booking date.
5. The client selects the preferred available time slot.
6. The system validates that the selected date and time remain available.
7. The selected booking date and time are associated with the booking request.
8. The client continues completing the booking request.

#### Alternative Flows

**A1 — Selected time slot is no longer available**

1. The selected date or time becomes unavailable before confirmation.
2. The system informs the client that the selected time is unavailable.
3. The system displays the updated availability.
4. The client selects another available date and time.

#### Postconditions

- A preferred booking date and time have been selected.
- The selected schedule is associated with the booking request.

#### Related Functional Requirements

- FR-023 — Select an Available Booking Date and Time

#### Related Business Process

- BP-07 — Booking Management

---

### UC-023 — Update Booking Request

#### Goal

Allow the client to update a submitted booking request before the booking has been confirmed.

#### Primary Actor

Client

#### Supporting Actors

None

#### Trigger

The client decides to modify a previously submitted booking request.

#### Preconditions

- The booking request exists.
- The booking has not yet been confirmed.
- The client accesses the booking through the booking tracking link.

#### Main Success Scenario

1. The client opens the booking tracking page using the booking tracking link.
2. The system displays the current booking request information.
3. The client updates one or more booking request details.
4. The client submits the updated booking request.
5. The system validates the updated information.
6. The system stores the updated booking request.
7. The system records the updated booking information.
8. The updated booking request becomes available for photographer review.

#### Alternative Flows

**A1 — Validation fails**

1. The updated booking information is incomplete or invalid.
2. The system displays the validation errors.
3. The client corrects the information.
4. The client resubmits the booking request.

**A2 — Booking has already been confirmed**

1. The client attempts to update a confirmed booking.
2. The system informs the client that booking requests can no longer be modified after confirmation.
3. The booking request remains unchanged.

#### Postconditions

- The booking request has been successfully updated.
- The latest booking request information is stored.
- The updated booking request is available for photographer review.

#### Related Functional Requirements

- FR-024 — Update Booking Request

#### Related Business Process

- BP-07 — Booking Management

---

### UC-024 — Cancel Booking Request

#### Goal

Allow the client to cancel a submitted booking request before the booking has been confirmed.

#### Primary Actor

Client

#### Supporting Actors

None

#### Trigger

The client decides to withdraw a previously submitted booking request.

#### Preconditions

- The booking request exists.
- The booking has not yet been confirmed.
- The client accesses the booking through the booking tracking link.

#### Main Success Scenario

1. The client opens the booking tracking page using the booking tracking link.
2. The system displays the current booking request information.
3. The client chooses to cancel the booking request.
4. The system requests cancellation confirmation.
5. The client confirms the cancellation.
6. The system cancels the booking request.
7. The system records the cancellation.
8. The booking request is no longer available for further processing.

#### Alternative Flows

**A1 — Client aborts the cancellation**

1. The client chooses to cancel the operation instead of confirming the cancellation.
2. The system leaves the booking request unchanged.

**A2 — Booking has already been confirmed**

1. The client attempts to cancel a confirmed booking.
2. The system informs the client that booking requests can no longer be cancelled after confirmation.
3. The booking remains unchanged.

#### Postconditions

- The booking request has been cancelled.
- The cancellation has been recorded.
- The booking request is no longer subject to photographer review.

#### Related Functional Requirements

- FR-025 — Cancel Booking Request

#### Related Business Process

- BP-07 — Booking Management

---

### UC-025 — Request Additional Booking Information

#### Goal

Allow the photographer to request additional booking information required to evaluate a booking request before making a booking decision.

#### Primary Actor

Photographer

#### Supporting Actors

Email Service

#### Trigger

The photographer determines that the submitted booking request requires additional information.

#### Preconditions

- A booking request exists.
- The booking request has not been confirmed.
- The photographer is reviewing the booking request.

#### Main Success Scenario

1. The photographer reviews the booking request.
2. The photographer requests additional booking information.
3. The system prepares an email containing; the requested information, the existing unique booking tracking link, and instructions asking the client to access the booking tracking page and provide or update the requested information.
4. The Email Service delivers the email to the client.
5. The system records that additional information has been requested.

#### Alternative Flows

**A1 — Information Obtained Outside the system**

1. The photographer obtains the required information through external communication (for example, a phone call).
2. The photographer updates the booking request directly.
3. The system stores the updated booking information.

#### Postconditions

- The client has been notified that additional information is required, or
- The booking request has been updated directly by the photographer.

#### Related Functional Requirements

- FR-026 — Provide Additional Booking Information
- FR-029 — Request Additional Booking Information

#### Related Business Process

- BP-07 — Booking Management

---

### UC-026 — Submit Additional Booking Information

#### Goal

Allow the client to provide the additional booking information requested by the photographer.

#### Primary Actor

Client

#### Supporting Actors

None

#### Trigger

The client receives an email requesting additional booking information.

#### Preconditions

- A booking request exists.
- Additional information has been requested by the photographer.
- The booking request has not been confirmed.
- The client has access to the booking tracking link.

#### Main Success Scenario

1. The client opens the booking tracking link received by email.
2. The system displays the current booking request together with the photographer's requested information.
3. The client reviews the request.
4. The client updates or completes the requested booking information.
5. The client submits the updated booking request.
6. The system validates the submitted information.
7. The system stores the updated booking request.
8. The system notifies the photographer that the requested information has been provided.

#### Alternative Flows

**A1 — Validation Failure**

1. The submitted information is invalid or incomplete.
2. The system displays the validation errors.
3. The client corrects the information and resubmits it.

**A2 — Booking Already Confirmed**

1. The client opens the booking tracking link after the booking has already been confirmed.
2. The system displays the booking information in read-only mode.
3. The use case ends.

#### Postconditions

- The booking request contains the additional information required by the photographer.
- The photographer is notified that the booking request has been updated.

#### Related Functional Requirements

- FR-026 — Provide Additional Booking Information

#### Related Business Process

- BP-07 — Booking Management

---

### UC-027 — Receive Booking Notifications

#### Goal

Allow the client to receive booking-related notifications throughout the booking lifecycle.

#### Primary Actor

Client

#### Supporting Actors

Email Service

#### Trigger

A booking event requiring client notification occurs.

#### Preconditions

- A booking request exists.
- The client's email address is available.
- A booking event requiring notification has occurred.

#### Main Success Scenario

1. A booking event requiring client notification occurs.
2. The system prepares the appropriate booking notification.
3. The Email Service delivers the notification to the client.
4. The client receives the notification.
5. The client reviews the notification.
6. If required, the client follows the instructions contained in the notification.

#### Alternative Flows

**A1 — Email Delivery Failure**

1. The Email Service fails to deliver the notification.
2. The system records the delivery failure.
3. The system may retry the delivery according to the notification policy.

#### Postconditions

- The booking notification has been delivered to the client, or
- The delivery failure has been recorded.

#### Related Functional Requirements

- FR-027 — Receive Booking Notifications

#### Related Business Process

- BP-07 — Booking Management

---

### UC-028 — Review Booking Requests

#### Goal

Allow the photographer to review submitted booking requests before deciding whether to approve, reject, or request additional information.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

A new booking request is available for review.

#### Preconditions

- A booking request exists.
- The booking request has not yet been approved or rejected.
- The photographer has access to the administrative dashboard.

#### Main Success Scenario

1. The photographer opens the booking management page.
2. The system displays all pending booking requests.
3. The photographer selects a booking request.
4. The system displays the complete booking request information, including; client information, requested services or package, preferred booking date and time, booking details, and previous communications when available.
5. The photographer reviews the booking request.
6. The use case ends.

#### Alternative Flows

**A1 — No Pending Booking Requests**

1. No booking requests are awaiting review.
2. The system informs the photographer that there are no pending booking requests.
3. The use case ends.

#### Postconditions

- The photographer has reviewed the selected booking request.
- The booking request remains awaiting the photographer's decision.

#### Related Functional Requirements

- FR-028 — Review Booking Requests

#### Related Business Process

- BP-07 — Booking Management

---

### UC-029 — Approve Booking Requests

#### Goal

Allow the photographer to approve a booking request so that the client can proceed with the required initial payment before the booking is confirmed.

#### Primary Actor

Photographer

#### Supporting Actors

Email Service

#### Trigger

The photographer decides to approve a reviewed booking request.

#### Preconditions

- A booking request exists.
- The booking request has been reviewed.
- All required booking information is available.
- The booking request has not been approved or rejected.

#### Main Success Scenario

1. The photographer approves the booking request.
2. The photographer may optionally add booking notes or instructions.
3. The system records the booking approval.
4. The system prepares the booking approval notification.
5. The notification includes; booking approval, payment instructions, the photographer's booking notes (when provided), and the existing unique booking tracking link.
6. The Email Service delivers the notification to the client.
7. The booking request enters the **Approved** state.
8. The use case ends.

#### Alternative Flows

**A1 — Booking Notes Not Provided**

1. The photographer approves the booking request without booking notes.
2. The system sends the booking approval and payment instructions only.

#### Postconditions

- The booking request has been approved.
- The client has received the booking approval and payment instructions.
- The booking is awaiting confirmation of the required initial payment.
- The booking has **not** yet become confirmed.

#### Related Functional Requirements

- FR-030 — Approve Booking Requests
- FR-033 — Receive Payment Instructions

#### Related Business Process

- BP-07 — Booking Management

---

### UC-030 — Reject Booking Requests

#### Goal

Allow the photographer to reject a booking request that cannot or should not proceed.

#### Primary Actor

Photographer

#### Supporting Actors

Email Service

#### Trigger

The photographer decides to reject a reviewed booking request.

#### Preconditions

- A booking request exists.
- The booking request has been reviewed.
- The booking request has not been approved or rejected.

#### Main Success Scenario

1. The photographer rejects the booking request.
2. The photographer may optionally provide a rejection reason or additional notes.
3. The system records the booking rejection.
4. The system prepares the booking rejection notification.
5. The notification includes; the booking rejection, the rejection reason or notes (when provided), and the existing unique booking tracking link for future reference.
6. The Email Service delivers the notification to the client.
7. The booking request enters the **Rejected** state.
8. The system sends a booking rejection notification to the client.
9. The use case ends.

#### Alternative Flows

**A1 — Rejection Reason Not Provided**

1. The photographer rejects the booking request without providing a rejection reason.
2. The system sends the booking rejection notification without additional comments.

#### Postconditions

- The booking request has been rejected.
- The client has been notified of the rejection.
- The booking request is closed without becoming a confirmed booking.
- The booking tracking page remains available to the client in read-only mode for historical reference.

#### Related Functional Requirements

- FR-031 — Reject Booking Requests
- FR-027 — Receive Booking Notifications

#### Related Business Process

- BP-07 — Booking Management

---

### UC-031 — Manage Confirmed Bookings

#### Goal

Allow the photographer to manage confirmed bookings throughout the service lifecycle until the booking is completed or otherwise closed.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens an existing confirmed booking.

#### Preconditions

- The booking has already been confirmed.
- The photographer is authenticated.
- The booking exists.

#### Main Success Scenario

1. The photographer opens a confirmed booking.
2. The system displays the complete booking information.
3. The photographer reviews the current booking details.
4. The photographer updates the booking whenever required.
5. The system validates the submitted changes.
6. The system saves the updated booking information.
7. The system records the modification.
8. The system notifies the client by email when the modification affects the client.
9. The updated booking information becomes available through the booking tracking page.
10. The photographer continues managing the booking until the photography service is completed or the booking is otherwise closed.

#### Alternative Flows

**A1 — Invalid Booking Information**

- The submitted booking information violates business rules.
- The system rejects the modification.
- The photographer corrects the information.

**A2 — Client Notification Not Delivered**

- The booking is updated successfully.
- Email delivery fails.
- The booking remains updated.
- The notification is queued for later delivery.

**A3 — Booking Becomes Completed**

- The photographer marks the booking as completed.
- The booking enters the Completed state.

#### Postconditions

- The confirmed booking reflects the latest agreed information.
- All booking modifications are recorded.
- The client has access to the latest booking information through the booking tracking page.
- When applicable, the client is notified of the booking changes.

#### Related Functional Requirements

- FR-032 — Manage Confirmed Bookings

#### Related Business Process

- BP-07 — Booking Management

---

## UC-05 — Payment

### UC-032 — Receive Payment Instructions

#### Goal

Allow the client to receive the payment instructions required to complete the initial booking payment after the booking request has been approved.

#### Primary Actor

Client

#### Supporting Actors

Email Service

#### Trigger

The photographer approves the booking request.

#### Preconditions

- The booking request has been approved.
- The booking has not yet been confirmed.
- The client has a valid email address.

#### Main Success Scenario

1. The photographer approves the booking request.
2. The system prepares the payment instructions.
3. The system includes any booking notes provided by the photographer.
4. The system sends the booking approval and payment instructions to the client by email.
5. The client receives the email.
6. The client reviews the payment instructions.

#### Alternative Flows

**A1 — Email Delivery Failure**

- The system cannot deliver the email.
- The system records the delivery failure.
- The email is queued for a later delivery attempt.

#### Postconditions

- The client has received (or is scheduled to receive) the payment instructions.
- The booking remains in the Approved state awaiting payment.

#### Related Functional Requirements

- FR-033 — Receive Payment Instructions

#### Related Business Process

- BP-07 — Booking Management

---

### UC-033 — Complete Booking Payments

#### Goal

Allow the client to complete one or more booking payments using the payment method agreed with the photographer.

#### Primary Actor

Client

#### Supporting Actors

None

#### Trigger

The client receives the payment instructions.

#### Preconditions

- The booking request has been approved.
- The client has received the payment instructions.
- A payment method has been agreed upon between the photographer and the client.

#### Main Success Scenario

1. The client reviews the payment instructions.
2. The client completes the required payment using the agreed payment method outside the system.
3. The client optionally informs the photographer that the payment has been completed.
4. The use case ends.

#### Alternative Flows

**A1 — Partial Payment**

- The client completes only part of the agreed amount.
- Additional payments may be completed later.

**A2 — Payment Delayed**

- The client does not complete the payment immediately.
- The booking remains awaiting payment confirmation.

#### Postconditions

- One or more payments have been completed outside the system.
- The photographer may proceed to record the received payment.

#### Related Functional Requirements

- FR-034 — Complete Booking Payments

#### Related Business Process

- BP-08 — Payment Management

---

### UC-034 — Record Payment

#### Goal

Allow the photographer to record a payment received from the client before confirming it.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer receives a payment from the client.

#### Preconditions

- The related booking exists.
- The photographer has received a payment from the client.
- The photographer is authenticated.

#### Main Success Scenario

1. The photographer opens the booking.
2. The photographer selects **Record Payment**.
3. The system displays the payment entry form.
4. The photographer enters the payment information.
5. The system validates the submitted information.
6. The system stores the payment as an unconfirmed payment record.
7. The payment becomes available for review and confirmation.

#### Alternative Flows

**A1 — Invalid Payment Information**

- The submitted payment information is incomplete or invalid.
- The system displays the validation errors.
- The photographer corrects the information.

#### Postconditions

- The payment record has been stored.
- The payment remains unconfirmed.
- The booking financial status remains unchanged until the payment is confirmed.

#### Related Functional Requirements

- FR-035 — Record Payment

#### Related Business Process

- BP-08 — Payment Management

---

### UC-035 — Confirm Payment

#### Goal

Allow the photographer to verify and confirm a recorded payment so that the booking financial status is updated accordingly.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer reviews a recorded payment.

#### Preconditions

- The payment has been recorded.
- The payment has not yet been confirmed.
- The related booking exists.
- The photographer is authenticated.

#### Main Success Scenario

1. The photographer opens the recorded payment.
2. The system displays the payment details.
3. The photographer verifies the payment information.
4. The photographer confirms the payment.
5. The system marks the payment as confirmed.
6. The system updates the booking payment history.
7. The system recalculates the booking balance.
8. If the confirmed payment satisfies the booking confirmation requirement, the system marks the booking as confirmed.
9. If the remaining booking balance becomes zero, the system marks the booking as fully paid.
10. The system generates an invoice for the confirmed payment.
11. The system sends the invoice to the client by email.

#### Alternative Flows

**A1 — Payment Verification Failed**

- The photographer determines that the recorded payment is incorrect or cannot be verified.
- The payment remains unconfirmed.
- The photographer may correct or remove the recorded payment.

**A2 — Invoice Delivery Failure**

- The payment is confirmed successfully.
- The invoice is generated successfully.
- Email delivery fails.
- The system records the delivery failure.
- The invoice remains valid and is queued for a later delivery attempt.

#### Postconditions

- The payment has been confirmed.
- The booking payment history has been updated.
- The booking balance reflects the confirmed payment.
- The booking financial status has been updated.
- An invoice has been generated for the confirmed payment.
- The client has received (or is scheduled to receive) the invoice.

#### Related Functional Requirements

- FR-036 — Confirm Payment
- FR-038 — Receive Invoices

#### Related Business Process

- BP-08 — Payment Management
- BP-09 — Invoice Management

---

### UC-036 — View Booking Payment History

#### Goal

Allow the photographer to review the complete payment history associated with a booking.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the payment history of a booking.

#### Preconditions

- The booking exists.
- The photographer is authenticated.

#### Main Success Scenario

1. The photographer opens a booking.
2. The photographer selects **View Payment History**.
3. The system retrieves all recorded payments associated with the booking.
4. The system displays the payment history in chronological order.
5. The photographer reviews the payment records, including; payment date, payment amount, payment method, payment status, invoice reference (when available), and remaining booking balance after each confirmed payment.
6. The use case ends.

#### Alternative Flows

**A1 — No Payments Recorded**

- No payment records exist for the booking.
- The system informs the photographer that no payments have been recorded.

#### Postconditions

- The photographer has reviewed the complete payment history of the booking.
- No booking or payment data is modified.

#### Related Functional Requirements

- FR-037 — View Booking Payment History

#### Related Business Process

- BP-08 — Payment Management

---

## UC-06 — Invoice

### UC-037 — Receive Invoices

#### Goal

Allow the client to receive an invoice by email after each confirmed payment.

#### Primary Actor

Client

#### Supporting Actors

Email Service

#### Trigger

A payment has been confirmed by the photographer.

#### Preconditions

- A payment has been confirmed.
- An invoice has been generated.
- The client has a valid email address.

#### Main Success Scenario

1. The system generates an invoice for the confirmed payment.
2. The system prepares the invoice email.
3. The system sends the invoice to the client's email address.
4. The client receives the invoice.
5. The client reviews the invoice.
6. The use case ends.

#### Alternative Flows

**A1 — Email Delivery Failure**

- The system cannot deliver the invoice.
- The system records the delivery failure.
- The invoice remains valid.
- The email is queued for a later delivery attempt.

#### Postconditions

- The client has received (or is scheduled to receive) the invoice.
- The invoice remains associated with the corresponding payment and booking.

#### Related Functional Requirements

- FR-038 — Receive Invoices

#### Related Business Process

- BP-09 — Invoice Management

---

### UC-038 — View Booking Invoices

#### Goal

Allow the photographer to review all invoices associated with a booking.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the invoices of a booking.

#### Preconditions

- The booking exists.
- The photographer is authenticated.

#### Main Success Scenario

1. The photographer opens a booking.
2. The photographer selects **View Invoices**.
3. The system retrieves all invoices associated with the booking.
4. The system displays the invoices in chronological order.
5. The photographer reviews the invoice information, including; invoice number, invoice issue date, related payment, payment amount, total amount paid, remaining balance, and invoice status.
6. The photographer may open an individual invoice to view its complete details.
7. The use case ends.

#### Alternative Flows

**A1 — No Invoices Available**

- No invoices exist for the booking.
- The system informs the photographer that no invoices are available.

#### Postconditions

- The photographer has reviewed the booking invoices.
- No invoice or booking information is modified.

#### Related Functional Requirements

- FR-039 — View Booking Invoices

#### Related Business Process

- BP-09 — Invoice Management

---

### UC-039 — Manage Invoices

#### Goal

Allow the photographer to manage invoices generated for booking payments.

#### Primary Actor

Photographer

#### Supporting Actors

Email Service

#### Trigger

The photographer selects an invoice for management.

#### Preconditions

- The invoice exists.
- The photographer is authenticated.

#### Main Success Scenario

1. The photographer opens an invoice.
2. The system displays the complete invoice details.
3. The photographer reviews the invoice.
4. The photographer may download or print the invoice.
5. The photographer may resend the invoice to the client's email address.
6. The system updates the invoice delivery history when the invoice is resent.
7. The use case ends.

#### Alternative Flows

**A1 — Email Delivery Failure**

- The invoice cannot be delivered.
- The system records the delivery failure.
- The invoice remains valid.
- The photographer may attempt to resend the invoice later.

#### Postconditions

- The invoice remains associated with the corresponding booking and payment.
- Any invoice delivery activity has been recorded.
- No financial information is modified.

#### Related Functional Requirements

- FR-040 — Manage Invoices

#### Related Business Process

- BP-09 — Invoice Management

---

## UC-07 — Business Administration

### UC-040 — Manage Business Information

#### Goal

Allow the photographer to manage the public business information presented throughout the system.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the business information management page.

#### Preconditions

- The photographer is authenticated.

#### Main Success Scenario

1. The photographer opens the business information management page.
2. The system displays the current business information.
3. The photographer updates the required business information.
4. The photographer saves the changes.
5. The system validates the submitted information.
6. The system stores the updated business information.
7. The updated information becomes immediately available throughout the system.

#### Alternative Flows

**A1 — Validation Failure**

- Mandatory information is missing or invalid.
- The system displays the validation errors.
- The photographer corrects the information and saves it again.

#### Postconditions

- The business information has been updated successfully.
- The updated information is visible throughout the system.

#### Related Functional Requirements

- FR-041 — Manage Business Information

#### Related Business Process

- BP-04 — Business Information Management

---

### UC-041 — Manage Contact Information

#### Goal

Allow the photographer to manage the public contact information used by visitors and clients to communicate with the photography business.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the contact information management page.

#### Preconditions

- The photographer is authenticated.

#### Main Success Scenario

1. The photographer opens the contact information management page.
2. The system displays the current contact information.
3. The photographer creates or updates the contact information.
4. The photographer saves the changes.
5. The system validates the submitted information.
6. The system stores the updated contact information.
7. The updated contact information becomes immediately available throughout the system.

#### Alternative Flows

**A1 — Validation Failure**

- Mandatory contact information is missing or invalid.
- The system displays the validation errors.
- The photographer corrects the information and saves it again.

#### Postconditions

- The contact information has been updated successfully.
- Visitors and clients can access the updated contact information throughout the system.

#### Related Functional Requirements

- FR-042 — Manage Contact Information

#### Related Business Process

- BP-04 — Business Information Management

---

### UC-042 — Manage Business Policies

#### Goal

Allow the photographer to manage the business policies presented to visitors and clients.

#### Primary Actor

Photographer

#### Supporting Actors

None

#### Trigger

The photographer opens the business policies management page.

#### Preconditions

- The photographer is authenticated.

#### Main Success Scenario

1. The photographer opens the business policies management page.
2. The system displays the current business policies.
3. The photographer creates or updates one or more business policies.
4. The photographer saves the changes.
5. The system validates the submitted information.
6. The system stores the updated business policies.
7. The updated business policies become immediately available throughout the system.

#### Alternative Flows

**A1 — Validation Failure**

- Required policy information is missing or invalid.
- The system displays the validation errors.
- The photographer corrects the information and saves it again.

#### Postconditions

- The business policies have been updated successfully.
- Visitors and clients can access the latest business policies throughout the system.

#### Related Functional Requirements

- FR-043 — Manage Business Policies

#### Related Business Process

- BP-04 — Business Information Management

---

