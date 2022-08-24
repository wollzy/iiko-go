### Note
Library is still under construction. Most of the methods are not implemented.

#### How to use
```golang
package main

import (
	"fmt"
	"log"
	"net/http"
	"time"

	"github.com/google/uuid"
	"github.com/iiko-go/iiko-go"
)

func main() {
	client, err := iiko.NewClient("API_LOGIN")
	if err != nil {
		log.Fatalln(err)
	}

	// You can set a custom http.Client for making iikoCloud API request.
	// By default, http.DefaultClient is used.
	client.SetHTTPClient(&http.Client{})

	// You can set a custom timeout for making iikoCloud API request.
	// By default, 15 seconds is used.
	client.SetTimeout(5 * time.Second)

	// You can set a custom refreshTokenInterval for iiko.Client.
	// refreshTokenInterval is the interval at which the iiko.Client wil try to get a new API token.
	// by default, 45 minutes is used.
	client.SetRefreshTokenInterval(30 * time.Minute)

	// Stoplist request.
	stoplistRequest := &iiko.StopListsRequest{
		OrganizationIDs: []uuid.UUID{
			uuid.MustParse("18C40D75-BA2E-4AFA-9DDE-28C46E7A7CEE"),
			uuid.MustParse("80DE4E4A-E2E1-45E6-928B-93628D35F8C2"),
		},
	}

	// iikoCloud API: /api/1/stop_lists
	res, err := client.StopLists(stoplistRequest)
	if err != nil {
		// Check if the error is IIKO API Error.
		if iikoError, ok := err.(*iiko.ErrorResponse); ok {
			fmt.Println(iikoError.StatusCode)
			fmt.Println(iikoError.CorrelationID)
			fmt.Println(iikoError.ErrorDescription)
			fmt.Println(iikoError.ErrorField)
			return
		} else {
			log.Fatalln(err)
		}
	}

	// You can also use custom options.
	// For example, iiko.WithCustomTimeout() will set a custom timeout for one API request.
	res, err = client.StopLists(stoplistRequest, iiko.WithCustomTimeout(5*time.Second))
	if err != nil {
		log.Fatalln(err)
	}

	// Now we can work with IIKO response.

	fmt.Println(res.CorrelationID) // print OperationID.

	for _, terminalGroup := range res.TerminalGroupStopLists {
		fmt.Println(terminalGroup.OrganizationID) // print OrganizationID.

		for _, product := range terminalGroup.Items {
			fmt.Println(product.ProductID) // print ProductID in stoplist.
		}
	}
}
```
#### Feature matrix
- [x] /access_token
- [x] /organizations
- [x] /cancel_causes
- [x] /combo/get_combos_info
- [x] /commands/status
- [x] /deliveries/order_types
- [x] /discounts
- [x] /nomenclature
- [x] /notifications/send
- [x] /organizations
- [x] /payment_types
- [x] /removal_types
- [x] /stop_lists
- [x] /terminal_groups/is_alive
- [x] /tips_types
- [ ] /cities
- [ ] /streets/by_city
- [ ] /deliveries/create
- [ ] /deliveries/update_order_problem
- [ ] /deliveries/update_order_delivery_status
- [ ] /deliveries/update_order_courier
- [ ] /deliveries/add_items
- [ ] /deliveries/close
- [ ] /deliveries/cancel
- [ ] /deliveries/change_complete_before
- [ ] /deliveries/change_delivery_point
- [ ] /deliveries/change_service_type
- [ ] /deliveries/change_payments
- [ ] /deliveries/change_comment
- [ ] /deliveries/print_delivery_bill
- [ ] /deliveries/by_id
- [ ] /deliveries/by_delivery_date_and_status
- [ ] /deliveries/by_revision
- [ ] /deliveries/by_delivery_date_and_phone
- [ ] /deliveries/by_delivery_date_and_source_key_and_filter
- [ ] /deliveries/drafts/by_id
- [ ] /deliveries/drafts/by_filter
- [ ] /deliveries/drafts/save
- [ ] /deliveries/drafts/commit
- [ ] /delivery_restrictions
- [ ] /delivery_restrictions/update
- [ ] /delivery_restrictions/allowed
- [ ] /employees/couriers/locations/by_time_offset
- [ ] /employees/couriers
- [ ] /employees/couriers/by_role
- [ ] /employees/couriers/active_location/by_terminal
- [ ] /employees/couriers/active_location
- [ ] /marketing_sources
- [ ] /order/create
- [ ] /order/by_id
- [ ] /order/by_table
- [ ] /order/add_items
- [ ] /order/close
- [ ] /order/change_payments
- [ ] /reserve/available_organizations
- [ ] /reserve/available_terminal_groups
- [ ] /reserve/available_restaurant_sections
- [ ] /reserve/restaurant_sections_workload
- [ ] /reserve/create
- [ ] /reserve/status_by_id
- [ ] /webhooks/settings
- [ ] /webhooks/update_settings
- [ ] /loyalty/iiko/get_customer
- [ ] /loyalty/iiko/calculate_checkin
- [ ] /loyalty/iiko/get_manual_conditions
- [ ] /combo/calculate_combo_price
- [ ] /regions
