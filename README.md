#### Step 1
Test Classifications: 
- Inputs: Reservation information (check-in/checkout dates, # of guests)
- Conditions: (i) Reservations must be between 1 - 31 nights, (ii) Reservations must include 1 - 12 guests, (iii) The check-in date must be before the checkout date, (iv) The requested date range must have every night available
- Calculated Values: (i) Base reservation costs, (ii) one-time cleaning fee, (iii) guest fees, (iv) Nights calculation (checkout date - check-in date)

Ranges and Conditions: 
(i) Reservation Nights: 
- Invalid: < 1 
- Valid: 1 - 31
- Invalid: > 31
(ii) Reservation Guests: 
- Invalid: < 1
- Valid: 1 - 12
- Invalid: > 12
(iii) Check-in/Checkout dates
- Invalid: Checkout is before check-in 
- Valid: Check-in is before checkout
- Invalid: Check-in and checkout are in the same date
(iv) Requested date ranges: 
- Valid: every night in the date range is available
- Invalid: one or more nights in the date range isn't available  


#### Step 2
(i) Reservation Nights: 
| Classification          | Partition             | Range or Condition  | Expected Result              |
| ----------------------- | --------------------- | ------------------- | ---------------------------- |
| # of reservation nights | Invalid, lower bound  | Less than 1 night   | `ValueError`                 |
| # of reservation nights | Valid                 | 1–31 nights         | Allowed to make reservation  |
| # of reservation nights | Invalid, upper bound  | More than 31 nights | `ValueError`                 |

(ii) Reservation Guests: 
| Classification          | Partition            | Range or Condition  | Expected Result              |
| ----------------------- | -------------------- | ------------------- | ---------------------------- |
| # of reservation guests | Invalid, lower bound | Less than 1 guest   | `ValueError`                 |
| # of reservation guests | Valid                | 1-12 guests         | Allowed to make reservation  |
| # of reservation guests | Invalid, upper bound | More than 12 guests | `ValueError`                 |

(iii) Check-in/Checkout dates: 
| Classification                | Partition|             Range or Condition             | Expected Result              |
| ----------------------------- | -------- | ------------------------------------------ | ---------------------------- |
| Reservation check-in/checkout | Invalid  | Checkout is before check-in                | `ValueError`                 |
| Reservation check-in/checkout | Valid    | Check-in is before checkout                | Allowed to make reservation  |
| Reservation check-in/checkout | Invalid  | Check-in and checkout are in the same date | `ValueError`                 |

(iv) Availability
| Classification                | Partition|             Range or Condition                      | Expected Result              |
| ----------------------------- | -------- | --------------------------------------------------- | ---------------------------- |
| Requested date ranges         | Valid    | Every night in the date range is available          | Allowed to make reservation  |
| Requested date ranges         | Invalid  | One or more nights in the date range are unavailable| `UnavailableError`           |

#### Step 3
TC01, Invalid Number of Reservation Nights (Upper Bound)
- **Classification:** # of reservation nights
- **Partition:** Invalid, upper bound
- **Input:** `37` nights
- **Expected Result:** The reservation is rejected, reservation information raises a `ValueError` 

TC02, Valid Number of Reservation Guests 
- **Classification:** # of reservation guests
- **Partition:** Valid
- **Input:** `11` guests  
- **Expected Result:** Allowed to make reservation.

TC03, Valid check-in/checkout reservation dates 
- **Classification:** Reservation check-in/checkout
- **Partition:** Valid
- **Input:** Check-in is before checkout 
- **Expected Result:** Allowed to make reservation.

TC04, Invalid requested date range
- **Classification:** Requested date ranges   
- **Partition:** Invalid
- **Input:** One or more nights in the date range are unavailable 
- **Expected Result:**  The reservation is rejected, resulting in a `UnavailableError`
