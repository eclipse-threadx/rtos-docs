---
title: Chapter 9 - USBX trace events
description: This chapter contains a description of the USBX events displayed by TraceX. 
---

# Chapter 9 - USBX trace events

This chapter contains a description of the USBX events displayed by
TraceX. 

## List of Events and Icons

The following is a list of USBX events displayed by TraceX.

| Icon                             | Meaning                               |
| -------------------------------- | ------------------------------------- |
| ![Device Class C D C Activate icon](./media/user-guide/usbx-events/image1.png)    | **Device Class Cdc Activate** *(ux_device_class_cdc_activate)* |
| ![Device Class C D C Deactivate icon](./media/user-guide/usbx-events/image2.png)    | **Device Class Cdc Deactivate** *(ux_device_class_cdc_deactivate)* |
| ![Device Class C D C Read icon](./media/user-guide/usbx-events/image3.png)    | **Device Class Cdc Read** *(ux_device_class_cdc_read)* |
| ![Device Class C D C Write icon](./media/user-guide/usbx-events/image4.png)    | **Device Class Cdc Write** *(ux_device_class_cdc_write)* |
| ![Device Class Dpump Activate icon](./media/user-guide/usbx-events/image5.png)    | **Device Class Dpump Activate** *(ux_device_class_dpump_activate)* |
| ![Device Class Dpump Deactivate icon](./media/user-guide/usbx-events/image6.png)    | **Device Class Dpump Deactivate** *(ux_device_class_dpump_deactivate)* |
| ![Device Class Dpump Read icon](./media/user-guide/usbx-events/image7.png)    | **Device Class Dpump Read** *(ux_device_class_dpump_read)* |
| ![Device Class Dpump Write icon](./media/user-guide/usbx-events/image8.png)    | **Device Class Dpump Write** *(ux_device_class_dpump_write)* |
| ![Device Class Hid Activate icon](./media/user-guide/usbx-events/image9.png)    | **Device Class Hid Activate** *(ux_device_class_hid_activate)* |
| ![Device Class Hid Deactivate icon](./media/user-guide/usbx-events/image10.png)    | **Device Class Hid Deactivate** *(ux_device_class_hid_deactivate)* |
| ![Device Class Hid Descriptor Send icon](./media/user-guide/usbx-events/image11.png)    | **Device Class Hid Descriptor Send** *(ux_device_class_hid_descriptor_send)* |
| ![Device Class Hid Event Get icon](./media/user-guide/usbx-events/image12.png)    | **Device Class Hid Event Get** *(ux_device_class_hid_event_get)* |
| ![Device Class Hid Event Set icon](./media/user-guide/usbx-events/image13.png)    | **Device Class Hid Event Set** *(ux_device_class_hid_event_set)* |
| ![Device Class Hid Report Get icon](./media/user-guide/usbx-events/image14.png)    | **Device Class Hid Report Get** *(ux_device_class_hid_report_get)* |
| ![Device Class Hid Report Set icon](./media/user-guide/usbx-events/image15.png)    | **Device Class Hid Report Set** *(ux_device_class_hid_report_set)* |
| ![Device Class Pima Activate icon](./media/user-guide/usbx-events/image16.png)    | **Device Class Pima Activate** *(ux_device_class_pima_activate)* |
| ![Device Class Pima Deactivate icon](./media/user-guide/usbx-events/image17.png)    | **Device Class Pima Deactivate** *(ux_device_class_pima_deactivate)* |
| ![Device Class Pima Device Info Send icon](./media/user-guide/usbx-events/image18.png)    | **Device Class Pima Device Info Send** *(ux_device_class_pima_device_info_send)* |
| ![Device Class Pima Event Get icon](./media/user-guide/usbx-events/image19.png)    | **Device Class Pima Event Get** *(ux_device_class_pima_event_get)* |
| ![Device Class Pima Event Set icon](./media/user-guide/usbx-events/image20.png)    | **Device Class Pima Event Set** *(ux_device_class_pima_event_set)* |
| ![Device Class Pima Object Add icon](./media/user-guide/usbx-events/image21.png)    | **Device Class Pima Object Add** *(ux_device_class_pima_object_add)* |
| ![Device Class Pima Object Data Get icon](./media/user-guide/usbx-events/image22.png)    | **Device Class Pima Object Data Get** *(ux_device_class_pima_object_data_get)* |
| ![Device Class Pima Object Data Send icon](./media/user-guide/usbx-events/image23.png)    | **Device Class Pima Object Data Send** *(ux_device_class_pima_object_data_send)* |
| ![Device Class Pima Object Delete icon](./media/user-guide/usbx-events/image24.png)    | **Device Class Pima Object Delete** *(ux_device_class_pima_object_delete)* |
| ![Device Class Pima Object Handles Send icon](./media/user-guide/usbx-events/image25.png)    | **Device Class Pima Object Handles Send** *(ux_device_class_pima_object_handles_send)* |
| ![Device Class Pima Object Info Get icon](./media/user-guide/usbx-events/image26.png)    | **Device Class Pima Object Info Get** *(ux_device_class_pima_object_info_get)* |
| ![Device Class Pima Object Info Send icon](./media/user-guide/usbx-events/image27.png)    | **Device Class Pima Object Info Send** *(ux_device_class_pima_object_info_send)* |
| ![Device Class Pima Objects Number Send icon](./media/user-guide/usbx-events/image28.png)    | **Device Class Pima Objects Number Send** *(ux_device_class_pima_objects_number_send)* |
| ![Device Class Pima Partial Object Data Get icon](./media/user-guide/usbx-events/image29.png)    | **Device Class Pima Partial Object Data Get** *(ux_device_class_pima_partial_object_data_get)* |
| ![Device Class Pima Response Send icon](./media/user-guide/usbx-events/image30.png)    | **Device Class Pima Response Send** *(ux_device_class_pima_response_send)*|
| ![Device Class Pima Storage I D Send icon](./media/user-guide/usbx-events/image31.png)    | **Device Class Pima Storage Id Send** *(ux_device_class_pima_storage_id_send)* |
| ![Device Class Pima Storage Info Send icon](./media/user-guide/usbx-events/image32.png)    | **Device Class Pima Storage Info Send** *(ux_device_class_pima_storage_info_send)* |
| ![Device Class R N D I S Activate icon](./media/user-guide/usbx-events/image33.png)    | **Device Class Rndis Activate** *(ux_device_class_rndis_activate)* |
| ![Device Class R N D I S Deactivate icon](./media/user-guide/usbx-events/image34.png)    | **Device Class Rndis Deactivate** *(ux_device_class_rndis_deactivate)* |
| ![Device Class R N D I S Message Keep Aliveicon](./media/user-guide/usbx-events/image35.png)    | **Device Class Rndis Message Keep Alive** *(ux_device_class_rndis_msg_keep_alive)* |
| ![Device Class R N D I S Message Query icon](./media/user-guide/usbx-events/image36.png)    | **Device Class Rndis Message Query** *(ux_device_class_rndis_msg_query)* |
| ![Device Class R N D I S Message Reset icon](./media/user-guide/usbx-events/image37.png)    | **Device Class Rndis Message Reset** *(ux_device_class_rndis_msg_reset)* |
| ![Device Class R N D I S Message Set icon](./media/user-guide/usbx-events/image38.png)    | **Device Class Rndis Message Set** *(ux_device_class_rndis_msg_set)* |
| ![Device Class R N D I S Packet Receive icon](./media/user-guide/usbx-events/image39.png)    | **Device Class Rndis Packet Receive** *(ux_device_class_rndis_packet_receive)* |
| ![Device Class R N D I S Packet Transmit icon](./media/user-guide/usbx-events/image40.png)    | **Device Class Rndis Packet Transmit** *(ux_device_class_rndis_packet_transmit)* |
| ![Device Class Storage Activate icon](./media/user-guide/usbx-events/image41.png)    | **Device Class Storage Activate** *(ux_device_class_storage_activate)* |
| ![Device Class Storage Deactivate icon](./media/user-guide/usbx-events/image42.png)    | **Device Class Storage Deactivate** *(ux_device_class_storage_deactivate)* |
| ![Device Class Storage Format icon](./media/user-guide/usbx-events/image43.png)    | **Device Class Storage Format** *(ux_device_class_storage_format)* |
| ![Device Class Storage Inquiry icon](./media/user-guide/usbx-events/image44.png)    | **Device Class Storage Inquiry** *(ux_device_class_storage_inquiry)* |
| ![Device Class Storage Mode Select icon](./media/user-guide/usbx-events/image45.png)    | **Device Class Storage Mode Select** *(ux_device_class_storage_mode_select)* |
| ![Device Class Storage Mode Sense icon](./media/user-guide/usbx-events/image46.png)    | **Device Class Storage Mode Sense** *(ux_device_class_storage_mode_sense)* |
| ![Device Class Storage Prevent Allow Media Removal icon](./media/user-guide/usbx-events/image47.png)    | **Device Class Storage Prevent Allow Media Removal** *(ux_device_class_storage_prevent_allow_media_removal)* |
| ![Device Class Storage Read icon](./media/user-guide/usbx-events/image48.png)    | **Device Class Storage Read** *(ux_device_class_storage_read)* |
| ![Device Class Storage Read Capacity icon](./media/user-guide/usbx-events/image49.png)    | **Device Class Storage Read Capacity** *(ux_device_class_storage_read_capacity)* |
| ![Device Class Storage Read Format Capacity icon](./media/user-guide/usbx-events/image50.png)    | **Device Class Storage Read Format Capacity** *(ux_device_class_storage_read_format_capacity)* |
| ![Device Class Storage Read TOC icon](./media/user-guide/usbx-events/image51.png)    | **Device Class Storage Read TOC** *(ux_device_class_storage_read_toc)* |
| ![Device Class Storage Request Sense icon](./media/user-guide/usbx-events/image52.png)    | **Device Class Storage Request Sense** *(ux_device_class_storage_request_sense)* |
| ![Device Class Storage Start Stop icon](./media/user-guide/usbx-events/image53.png)    | **Device Class Storage Start Stop** *(ux_device_class_storage_start_stop)* |
| ![Device Class Storage Test Ready icon](./media/user-guide/usbx-events/image54.png)    | **Device Class Storage Test Ready** *(ux_device_class_storage_test_ready)* |
| ![Device Class Storage Verify icon](./media/user-guide/usbx-events/image55.png)    | **Device Class Storage Verify** *(ux_device_class_storage_verify)* |
| ![Device Class Storage Write icon](./media/user-guide/usbx-events/image56.png)    | **Device Class Storage Write** *(ux_device_class_storage_write)* |
| ![Device Stack Alternate Setting Get icon](./media/user-guide/usbx-events/image57.png)    | **Device Stack Alternate Setting Get** *(ux_device_stack_alternate_setting_get)* |
| ![Device Stack Alternate Setting Set icon](./media/user-guide/usbx-events/image58.png)    | **Device Stack Alternate Setting Set** *(ux_device_stack_alternate_setting_set)* |
| ![Device Stack Class Register icon](./media/user-guide/usbx-events/image59.png)    | **Device Stack Class Register** *(ux_device_stack_class_register)* |
| ![Device Stack Clear Feature icon](./media/user-guide/usbx-events/image60.png)    | **Device Stack Clear Feature** *(ux_device_stack_clear_feature)* |
| ![Device Stack Configuration Get icon](./media/user-guide/usbx-events/image61.png)    | **Device Stack Configuration Get** *(ux_device_stack_configuration_get)* |
| ![Device Stack Configuration Set icon](./media/user-guide/usbx-events/image62.png)    | **Device Stack Configuration Set** *(ux_device_stack_configuration_set)* |
| ![Device Stack Connect icon](./media/user-guide/usbx-events/image63.png)    | **Device Stack Connect** *(ux_device_stack_connect)* |
| ![Device Stack Descriptor Send icon](./media/user-guide/usbx-events/image64.png)    | **Device Stack Descriptor Send** *(ux_device_stack_descriptor_send)* |
| ![Device Stack Disconnect icon](./media/user-guide/usbx-events/image65.png)    | **Device Stack Disconnect** *(ux_device_stack_disconnect)* |
| ![Device Stack Endpoint Stall icon](./media/user-guide/usbx-events/image66.png)    | **Device Stack Endpoint Stall** *(ux_device_stack_endpoint_stall)* |
| ![Device Stack Get Status icon](./media/user-guide/usbx-events/image67.png)    | **Device Stack Get Status** *(ux_device_stack_get_status)* |
| ![Device Stack Host Wakeup icon](./media/user-guide/usbx-events/image68.png)    | **Device Stack Host Wakeup** *(ux_device_stack_host_wakeup)* |
| ![Device Stack Initialize icon](./media/user-guide/usbx-events/image69.png)    | **Device Stack Initialize** *(ux_device_stack_initialize)* |
| ![Device Stack Interface Delete icon](./media/user-guide/usbx-events/image70.png)    | **Device Stack Interface Delete** *(ux_device_stack_interface_delete)* |
| ![Device Stack Interface Get icon](./media/user-guide/usbx-events/image71.png)    | **Device Stack Interface Get** *(ux_device_stack_interface_get)* |
| ![Device Stack Interface Set icon](./media/user-guide/usbx-events/image72.png)    | **Device Stack Interface Set** *(ux_device_stack_interface_set)* |
| ![Device Stack Set Feature icon](./media/user-guide/usbx-events/image73.png)    | **Device Stack Set Feature** *(ux_device_stack_set_feature)* |
| ![Device Stack Transfer Abort icon](./media/user-guide/usbx-events/image74.png)    | **Device Stack Transfer Abort** *(ux_device_stack_transfer_abort)* |
| ![*Device Stack Transfer All Request Abort icon](./media/user-guide/usbx-events/image75.png)    | **Device Stack Transfer All Request Abort** *(ux_device_stack_transfer_all_request_abort)* |
| ![Device Stack Transfer Request icon](./media/user-guide/usbx-events/image76.png)    | **Device Stack Transfer Request** *(ux_device_stack_transfer_request)* |
| ![Host Class Asix Activate icon](./media/user-guide/usbx-events/image77.png)    | **Host Class Asix Activate** *(ux_host_class_asix_activate)* |
| ![Host Class Asix Deactivate icon](./media/user-guide/usbx-events/image78.png)    | **Host Class Asix Deactivate** *(ux_host_class_asix_deactivate)* |
| ![Host Class Asix Interrupt Notification icon](./media/user-guide/usbx-events/image79.png)    | **Host Class Asix Interrupt Notification** *(ux_host_class_asix_interrupt_notification)* |
| ![Host Class Asix Read icon](./media/user-guide/usbx-events/image80.png)    | **Host Class Asix Read** *(ux_host_class_asix_read)* |
| ![Host Class Asix Write icon](./media/user-guide/usbx-events/image81.png)    | **Host Class Asix Write** *(ux_host_class_asix_write)* |
| ![Host Class Audio Activate icon](./media/user-guide/usbx-events/image82.png)    | **Host Class Audio Activate** *(ux_host_class_audio_activate)* |
| ![Host Class Audio Control Value Get icon](./media/user-guide/usbx-events/image83.png)    | **Host Class Audio Control Value Get** *(ux_host_class_audio_control_value_get)* |
| ![Host Class Audio Control Value Set icon](./media/user-guide/usbx-events/image84.png)    | **Host Class Audio Control Value Set** *(ux_host_class_audio_control_value_set)* |
| ![Host Class Audio Deactivate icon](./media/user-guide/usbx-events/image85.png)    | **Host Class Audio Deactivate** *(ux_host_class_audio_deactivate)* |
| ![Host Class Audio Read icon](./media/user-guide/usbx-events/image86.png)    | **Host Class Audio Read** *(ux_host_class_audio_read)* |
| ![Host Class Audio Streaming Sampling Get icon](./media/user-guide/usbx-events/image87.png)    | **Host Class Audio Streaming Sampling Get** *(ux_host_class_audio_streaming_sampling_get)* |
| ![Host Class Audio Streaming Sampling Set icon](./media/user-guide/usbx-events/image88.png)    | **Host Class Audio Streaming Sampling Set** *(ux_host_class_audio_streaming_sampling_set)* |
| ![Host Class Audio Write icon](./media/user-guide/usbx-events/image89.png)    | **Host Class Audio Write** *(ux_host_class_audio_write)* |
| ![Host Class C D C A C M Activate icon](./media/user-guide/usbx-events/image90.png)    | **Host Class Cdc Acm Activate** *(ux_host_class_cdc_acm_activate)* |
| ![Host Class C D C A C M Deactivate icon](./media/user-guide/usbx-events/image91.png)    | **Host Class Cdc Acm Deactivate** *(ux_host_class_cdc_acm_deactivate)* |
| ![Host Class C D C A C M I O C T L In Pipe icon](./media/user-guide/usbx-events/image92.png)    | **Host Class Cdc Acm Ioctl Abort In Pipe** *(ux_host_class_cdc_acm_ioctl_abort_in_pipe)* |
| ![Host Class C D C A C M I O C T L Abort Out Pipe icon](./media/user-guide/usbx-events/image93.png)    | **Host Class Cdc Acm Ioctl Abort Out Pipe** *(ux_host_class_cdc_acm_ioctl_abort_out_pipe)* |
| ![Host Class C D C A C M I O C T L Get Device Status icon](./media/user-guide/usbx-events/image94.png)    | **Host Class Cdc Acm Ioctl Get Device Status** *(ux_host_class_cdc_acm_ioctl_get_device_status)* |
| ![Host Class C D C A C M I O C T L Get Line Coding icon](./media/user-guide/usbx-events/image95.png)    | **Host Class Cdc Acm Ioctl Get Line Coding** *(ux_host_class_cdc_acm_ioctl_get_line_coding)* |
| ![Host Class C D C A C M I O C T L Notification Callback icon](./media/user-guide/usbx-events/image96.png)    | **Host Class Cdc Acm Ioctl Notification Callback** *(ux_host_class_cdc_acm_ioctl_notification_callback)* |
| ![Host Class C D C A C M I O C T L Send Break icon](./media/user-guide/usbx-events/image97.png)    | **Host Class Cdc Acm Ioctl Send Break** *(ux_host_class_cdc_acm_ioctl_send_break)* |
| ![Host Class C D C A C M I O C T L Set Line Coding icon](./media/user-guide/usbx-events/image98.png)    | **Host Class Cdc Acm Ioctl Set Line Coding** *(ux_host_class_cdc_acm_ioctl_set_line_coding)* |
| ![Host Class C D C A C M I O C T L Set Line State icon](./media/user-guide/usbx-events/image99.png)    | **Host Class Cdc Acm Ioctl Set Line State** *(ux_host_class_cdc_acm_ioctl_set_line_state)* |
| ![Host Class C D C A C M Read icon](./media/user-guide/usbx-events/image100.png)    | **Host Class Cdc Acm Read** *(ux_host_class_cdc_acm_read)* |
| ![Host Class C D C A C M Reception Start icon](./media/user-guide/usbx-events/image101.png)    | **Host Class Cdc Acm Reception Start** *(ux_host_class_cdc_acm_reception_start)* |
| ![Host Class C D C A C M Reception Stop icon](./media/user-guide/usbx-events/image102.png)    | **Host Class Cdc Acm Reception Stop** *(ux_host_class_cdc_acm_reception_stop)* |
| ![Host Class C D C A C M Write icon](./media/user-guide/usbx-events/image103.png)    | **Host Class Cdc Acm Write** *(ux_host_class_cdc_acm_write)* |
| ![Host Class Dpump Activate icon](./media/user-guide/usbx-events/image104.png)    | **Host Class Dpump Activate** *(ux_host_class_dpump_activate)* |
| ![Host Class Dpump Deactivate icon](./media/user-guide/usbx-events/image105.png)    | **Host Class Dpump Deactivate** *(ux_host_class_dpump_deactivate)* |
| ![Host Class Dpump Read icon](./media/user-guide/usbx-events/image106.png)    | **Host Class Dpump Read** *(ux_host_class_dpump_read)* |
| ![Host Class Dpump Write icon](./media/user-guide/usbx-events/image107.png)    | **Host Class Dpump Write** *(ux_host_class_dpump_write)* |
| ![Host Class Hid Activate icon](./media/user-guide/usbx-events/image108.png)    | **Host Class Hid Activate** *(ux_host_class_hid_activate)* |
| ![Host Class Hid Client Register icon](./media/user-guide/usbx-events/image109.png)    | **Host Class Hid Client Register** *(ux_host_class_hid_client_register)* |
| ![Host Class Hid Deactivate icon](./media/user-guide/usbx-events/image110.png)    | **Host Class Hid Deactivate** *(ux_host_class_hid_deactivate)* |
| ![Host Class Hid Idle Get icon](./media/user-guide/usbx-events/image111.png)    | **Host Class Hid Idle Get** *(ux_host_class_hid_idle_get)* |
| ![Host Class Hid Idle Set icon](./media/user-guide/usbx-events/image112.png)    | **Host Class Hid Idle Set** *(ux_host_class_hid_idle_set)* |
| ![Host Class Hid Keyboard Activate icon](./media/user-guide/usbx-events/image113.png)    | **Host Class Hid Keyboard Activate** *(ux_host_class_hid_keyboard_activate)* |
| ![Host Class Hid Keyboard Deactivate icon](./media/user-guide/usbx-events/image114.png)    | **Host Class Hid Keyboard Deactivate** *(ux_host_class_hid_keyboard_deactivate)* |
| ![Host Class Hid Mouse Activate icon](./media/user-guide/usbx-events/image115.png)    | **Host Class Hid Mouse Activate** *(ux_host_class_hid_mouse_activate)* |
| ![Host Class Hid Mouse Deactivate icon](./media/user-guide/usbx-events/image116.png)    | **Host Class Hid Mouse Deactivate** *(ux_host_class_hid_mouse_deactivate)* |
| ![Host Class Hid Remote Control Activate icon](./media/user-guide/usbx-events/image117.png)    | **Host Class Hid Remote Control Activate** *(ux_host_class_hid_remote_control_activate)* |
| ![Host Class Hid Remote Control Deactivate icon](./media/user-guide/usbx-events/image118.png)    | **Host Class Hid Remote Control Deactivate** *(ux_host_class_hid_remote_control_deactivate)* |
| ![Host Class Hid Report Get icon](./media/user-guide/usbx-events/image119.png)    | **Host Class Hid Report Get** *(ux_host_class_hid_report_get)* |
| ![Host Class Hid Report Set icon](./media/user-guide/usbx-events/image120.png)    | **Host Class Hid Report Set** *(ux_host_class_hid_report_set)* |
| ![Host Class Hub Activate icon](./media/user-guide/usbx-events/image121.png)    | **Host Class Hub Activate** *(ux_host_class_hub_activate)* |
| ![Host Class Hub Change Detect icon](./media/user-guide/usbx-events/image122.png)    | **Host Class Hub Change Detect** *(ux_host_class_hub_change_detect)* |
| ![*Host Class Hub Deactivate icon](./media/user-guide/usbx-events/image123.png)    | **Host Class Hub Deactivate** *(ux_host_class_hub_deactivate)* |
| ![Host Class Hub Port Change Connection Process icon](./media/user-guide/usbx-events/image124.png)    | **Host Class Hub Port Change Connection Process** *(ux_host_class_hub_port_change_connection_process)* |
| ![Host Class Hub Port Change Enable Process icon](./media/user-guide/usbx-events/image125.png)    | **Host Class Hub Port Change Enable Process** *(ux_host_class_hub_port_change_enable_process)* |
| ![Host Class Hub Port Change Over Current Process icon](./media/user-guide/usbx-events/image126.png)    | **Host Class Hub Port Change Over Current Process** *(ux_host_class_hub_port_change_over_current_process)* |
| ![Host Class Hub Port Change Reset Process icon](./media/user-guide/usbx-events/image127.png)    | **Host Class Hub Port Change Reset Process** *(ux_host_class_hub_port_change_reset_process)* |
| ![Host Class Hub Port Change Suspend Process icon](./media/user-guide/usbx-events/image128.png)    | **Host Class Hub Port Change Suspend Process** *(ux_host_class_hub_port_change_suspend_process)* |
| ![Host Class Pima Activate icon](./media/user-guide/usbx-events/image129.png)    | **Host Class Pima Activate** *(ux_host_class_prima_activate)* |
| ![Host Class Pima Deactivate icon](./media/user-guide/usbx-events/image130.png)    | **Host Class Pima Deactivate** *(ux_host_class_pima_deactivate)* |
| ![Host Class Pima Device Info Get icon](./media/user-guide/usbx-events/image131.png)    | **Host Class Pima Device Info Get** *(ux_host_class_pima_device_info_get)* |
| ![Host Class Pima Device Reset icon](./media/user-guide/usbx-events/image132.png)    | **Host Class Pima Device Reset** *(ux_host_class_pima_device_reset)* |
| ![Host Class Pima Notification icon](./media/user-guide/usbx-events/image133.png)    | **Host Class Pima Notification** *(ux_host_class_pima_notification)* |
| ![Host Class Pima Number Objects Get icon](./media/user-guide/usbx-events/image134.png)    | **Host Class Pima Number Objects Get** *(ux_host_class_pima_num_objects_get)* |
| ![Host Class Pima Object Close icon](./media/user-guide/usbx-events/image135.png)    | **Host Class Pima Object Close** *(ux_host_class_pima_object_close)* |
| ![Host Class Pima Object Copy icon](./media/user-guide/usbx-events/image136.png)    | **Host Class Pima Object Copy** *(ux_host_class_pima_object_copy)* |
| ![Host Class Pima Object Delete icon](./media/user-guide/usbx-events/image137.png)    | **Host Class Pima Object Delete** *(ux_host_class_pima_object_delete)* |
| ![Host Class Pima Object Get icon](./media/user-guide/usbx-events/image138.png)    | **Host Class Pima Object Get** *(ux_host_class_pima_object_get)* |
| ![Host Class Pima Object Info Get icon](./media/user-guide/usbx-events/image139.png)    | **Host Class Pima Object Info Get** *(ux_host_class_pima_object_info_get)* |
| ![Host Class Pima Object Info Send icon](./media/user-guide/usbx-events/image140.png)    | **Host Class Pima Object Info Send** *(ux_host_class_pima_object_info_send)* |
| ![Host Class Pima Object Move icon](./media/user-guide/usbx-events/image141.png)    | **Host Class Pima Object Move** *(ux_host_class_pima_object_move)* |
| ![Host Class Pima Object Send icon](./media/user-guide/usbx-events/image142.png)    | **Host Class Pima Object Send** *(ux_host_class_pima_object_send)* |
| ![Host Class Pima Object Transfer Abort icon](./media/user-guide/usbx-events/image143.png)    | **Host Class Pima Object Transfer Abort** *(ux_host_class_object_transfer_abort)* |
| ![Host Class Pima Read icon](./media/user-guide/usbx-events/image144.png)    | **Host Class Pima Read** *(ux_host_class_pima_read)* |
| ![Host Class Pima Request Cancel icon](./media/user-guide/usbx-events/image145.png)    | **Host Class Pima Request Cancel** *(ux_host_class_pima_request_cancel)* |
| ![Host Class Pima Session Close icon](./media/user-guide/usbx-events/image146.png)    | **Host Class Pima Session Close** *(ux_host_class_pima_session_close)* |
| ![Host Class Pima Session Open icon](./media/user-guide/usbx-events/image147.png)    | **Host Class Pima Session Open** *(ux_host_class_pima_session_open)* |
| ![Host Class Pima Storage Ids Get icon](./media/user-guide/usbx-events/image148.png)    | **Host Class Pima Storage Ids Get** *(ux_host_class_pima_storage_ids_get)* |
| ![Host Class Pima Storage Info Get icon](./media/user-guide/usbx-events/image149.png)    | **Host Class Pima Storage Info Get** *(ux_host_class_pima_storage_info_get)* |
| ![Host Class Pima Thumb Get icon](./media/user-guide/usbx-events/image150.png)    | **Host Class Pima Thumb Get** *(ux_host_class_pima_thumb_get)* |
| ![Host Class Pima Write icon](./media/user-guide/usbx-events/image151.png)    | **Host Class Pima Write** *(ux_host_class_pima_write)* |
| ![Host Class Printer Activate icon](./media/user-guide/usbx-events/image152.png)    | **Host Class Printer Activate** *(ux_host_class_printer_activate)* |
| ![Host Class Printer Deactivate icon](./media/user-guide/usbx-events/image153.png)    | **Host Class Printer Deactivate** *(ux_host_class_printer_deactivate)* |
| ![Host Class Printer Name Get icon](./media/user-guide/usbx-events/image154.png)    | **Host Class Printer Name Get** *(ux_host_class_printer_name_get)* |
| ![Host Class Printer Read icon](./media/user-guide/usbx-events/image155.png)    |  **Host Class Printer Read** *(ux_host_class_printer_read)* |
| ![Host Class Printer Soft Reset icon](./media/user-guide/usbx-events/image156.png)    | **Host Class Printer Soft Reset** *(ux_host_class_printer_soft_reset)* |
| ![Host Class Printer Status Get icon](./media/user-guide/usbx-events/image157.png)    | **Host Class Printer Status Get** *(ux_host_class_printer_status_get)* |
| ![Host Class Printer Write icon](./media/user-guide/usbx-events/image158.png)    | **Host Class Printer Write** *(ux_host_class_printer_write)* |
| ![Host Class Prolific Activate icon](./media/user-guide/usbx-events/image159.png)    | **Host Class Prolific Activate** *(ux_host_class_prolific_activate)* |
| ![Host Class Prolific Deactivate icon](./media/user-guide/usbx-events/image160.png)    | **Host Class Prolific Deactivate** *(ux_host_class_prolific_deactivate)* |
| ![Host Class Prolific I O C T L Abort In Pipe icon](./media/user-guide/usbx-events/image161.png)    | **Host Class Prolific Ioctl Abort In Pipe** *(ux_host_class_prolific_ioctl_abort_in_pipe)* |
| ![Host Class Prolific I O C T L Abort Out Pipe icon](./media/user-guide/usbx-events/image162.png)    | **Host Class Prolific Ioctl Abort Out Pipe** *(ux_host_class_prolific_ioctl_abort_out_pipe)* |
| ![Host Class Prolific I O C T L Get Device Status icon](./media/user-guide/usbx-events/image163.png)    | **Host Class Prolific Ioctl Get Device Status** *(ux_host_class_prolific_ioctl_get_device_status)* |
| ![Host Class Prolific I O C T L Get Line Coding icon](./media/user-guide/usbx-events/image164.png)    | **Host Class Prolific Ioctl Get Line Coding** *(ux_host_class_prolific_ioctl_get_line_coding)* |
| ![Host Class Prolific I O C T L Purge icon](./media/user-guide/usbx-events/image165.png)    | **Host Class Prolific Ioctl Purge** *(ux_host_class_prolific_ioctl_purge)* |
| ![Host Class Prolific I O C T L Report Device Status Change icon](./media/user-guide/usbx-events/image166.png)    | **Host Class Prolific Ioctl Report Device Status Change** *(ux_host_class_prolific_ioctl_report_device_status_change)* |
| ![Host Class Prolific I O C T L Send Break icon](./media/user-guide/usbx-events/image167.png)    | **Host Class Prolific Ioctl Send Break** *(ux_host_class_prolific_ioctl_send_break)* |
| ![Host Class Prolific I O C T L Set Line Coding icon](./media/user-guide/usbx-events/image168.png)    | **Host Class Prolific Ioctl Set Line Coding** *(ux_host_class_prolific_ioctl_set_line_coding)* |
| ![Host Class Prolific I O C T L Set Line State icon](./media/user-guide/usbx-events/image169.png)    | **Host Class Prolific Ioctl Set Line State** *(ux_host_class_prolific_ioctl_set_line_state)* |
| ![Host Class Prolific Read icon](./media/user-guide/usbx-events/image170.png)    | **Host Class Prolific Read** *(ux_host_class_prolific_read)* |
| ![Host Class Prolific Reception Start icon](./media/user-guide/usbx-events/image171.png)    | **Host Class Prolific Reception Start** *(ux_host_class_prolific_reception_start)* |
| ![Host Class Prolific Reception Stop icon](./media/user-guide/usbx-events/image172.png)    | **Host Class Prolific Reception Stop** *(ux_host_class_prolific_reception_stop)* |
| ![Host Class Prolific Write icon](./media/user-guide/usbx-events/image173.png)    | **Host Class Prolific Write** *(ux_host_class_prolific_write)* |
| ![Host Class Storage Activate icon](./media/user-guide/usbx-events/image174.png)    | **Host Class Storage Activate** *(ux_host_class_storage_activate)* |
| ![Host Class Storage Deactivate icon](./media/user-guide/usbx-events/image175.png)    | **Host Class Storage Deactivate** (*ux_host_class_storage_deactivate)* |
| ![Host Class Storage Media Capacity Get icon](./media/user-guide/usbx-events/image176.png)    | **Host Class Storage Media Capacity Get** *(ux_host_class_storage_media_capacity_get)* |
| ![Host Class Storage Media Format Capacity Get icon](./media/user-guide/usbx-events/image177.png)    | **Host Class Storage Media Format Capacity Get** *(ux_host_class_storage_media_format_capacity_get)* |
| ![Host Class Storage Media Mount icon](./media/user-guide/usbx-events/image178.png)    | **Host Class Storage Media Mount** (ux_host_class_storage_media_mount)* |
| ![Host Class Storage Media Open icon](./media/user-guide/usbx-events/image179.png)    | **Host Class Storage Media Open** *(ux_host_class_storage_media_open)* |
| ![Host Class Storage Media Read icon](./media/user-guide/usbx-events/image180.png)    | **Host Class Storage Media Read** *(ux_host_class_storage_media_read)* |
| ![Host Class Storage Media Write icon](./media/user-guide/usbx-events/image181.png)    | **Host Class Storage Media Write** *(ux_host_class_storage_media_write)* |
| ![Host Class Storage Request Sense icon](./media/user-guide/usbx-events/image182.png)    | **Host Class Storage Request Sense** *(ux_host_class_storage_request_sense)* |
| ![Host Class Storage Start Stop icon](./media/user-guide/usbx-events/image183.png)    | **Host Class Storage Start Stop** *(ux_host_class_storage_start_stop)* |
| ![Host Class Storage Unit Ready Test icon](./media/user-guide/usbx-events/image184.png)    | **Host Class Storage Unit Ready Test** *(ux_host_class_storage_activate)* |
| ![Host Stack Class Instance Create icon](./media/user-guide/usbx-events/image185.png)    | **Host Stack Class Instance Create** *(ux_host_stack_class_instance_create)* |
| ![Host Stack Class Instance Destroy icon](./media/user-guide/usbx-events/image186.png)    | **Host Stack Class Instance Destroy** *(ux_host_stack_class_instance_destroy)* |
| ![Host Stack Configuration Delete icon](./media/user-guide/usbx-events/image187.png)    | **Host Stack Configuration Delete** *(ux_host_stack_configuration_delete)* |
| ![Host Stack Configuration Enumerate icon](./media/user-guide/usbx-events/image188.png)    | **Host Stack Configuration Enumerate** *(ux_host_stack_configuration_enumerate)* |
| ![Host Stack Configuration Instance Create icon](./media/user-guide/usbx-events/image189.png)    | **Host Stack Configuration Instance Create** *(ux_host_stack_configuration_instance_create)* |
| ![Host Stack Configuration Instance Delete icon](./media/user-guide/usbx-events/image190.png)    | **Host Stack Configuration Instance Delete** *(ux_host_stack_configuration_instance_delete)* |
| ![Host Stack Configuration Set icon](./media/user-guide/usbx-events/image191.png)    | **Host Stack Configuration Set** *(ux_host_stack_configuration_set)* |
| ![Host Stack Device Address Set icon](./media/user-guide/usbx-events/image192.png)    | **Host Stack Device Address Set** *(ux_host_stack_device_set)* |
| ![Host Stack Device Configuration Get icon](./media/user-guide/usbx-events/image193.png)    | **Host Stack Device Configuration Get** *(ux_host_stack_device_configuration_get)* |
| ![Host Stack Device Configuration Select icon](./media/user-guide/usbx-events/image194.png)    | **Host Stack Device Configuration Select** *(ux_host_stack_device_configuration_select)* |
| ![Host Stack Device Descriptor Read icon](./media/user-guide/usbx-events/image195.png)    | **Host Stack Device Descriptor Read** *(ux_host_stack_device_descriptor_read)* |
| ![Host Stack Device Get icon](./media/user-guide/usbx-events/image196.png)    | **Host Stack Device Get** (ux_host_stack_device_get) |
| ![Host Stack Device Remove icon](./media/user-guide/usbx-events/image197.png)    | **Host Stack Device Remove** (ux_host_stack_device_get) |
| ![Host Stack Device Resource Free icon](./media/user-guide/usbx-events/image198.png)    | **Host Stack Device Resource Free** (ux_host_stack_device_resource_free) |
| ![Host Stack Endpoint Instance Create icon](./media/user-guide/usbx-events/image199.png)    | **Host Stack Endpoint Instance Create** (ux_host_stack_endpoint_instance_create) |
| ![Host Stack Endpoint Instance Delete icon](./media/user-guide/usbx-events/image200.png)    | **Host Stack Endpoint Instance Delete** (ux_host_stack_endpoint_instance_delete) |
| ![Host Stack Endpoint Reset icon](./media/user-guide/usbx-events/image201.png)    | **Host Stack Endpoint Reset** (ux_host_stack_endpoint_reset) |
| ![Host Stack Endpoint Transfer Abort icon](./media/user-guide/usbx-events/image202.png)    | **Host Stack Endpoint Transfer Abort** (ux_host_stack_endpoint_transfer_abort) |
| ![Host Stack Host Controller Register icon](./media/user-guide/usbx-events/image203.png)    | **Host Stack Host Controller Register** *(ux_host_stack_hcd_register)* |
| ![Host Stack Initialize icon](./media/user-guide/usbx-events/image204.png)    | **Host Stack Initialize** *(ux_host_stack_initialize)* |
| ![Host Stack Interface Endpoint Get icon](./media/user-guide/usbx-events/image205.png)    | **Host Stack Interface Endpoint Get** *(ux_host_stack_interface_endpoint_get)* |
| ![Host Stack Interface Instance Create icon](./media/user-guide/usbx-events/image206.png)    | **Host Stack Interface Instance Create** *(ux_host_stack_interface_instance_create)* |
| ![Host Stack Interface Instance Delete icon](./media/user-guide/usbx-events/image207.png)    | **Host Stack Interface Instance Delete** *(ux_host_stack_interface_instance_delete)* |
| ![Host Stack Interface Set icon](./media/user-guide/usbx-events/image208.png)    | **Host Stack Interface Set** *(ux_host_stack_interface_set)* |
| ![Host Stack Interface Setting Select icon](./media/user-guide/usbx-events/image209.png)    | **Host Stack Interface Setting Select** *(ux_host_stack_interface_setting_select)* |
| ![Host Stack New Configuration Create icon](./media/user-guide/usbx-events/image210.png)    | **Host Stack New Configuration Create** *(ux_host_stack_new_configuration_create)* |
| ![Host Stack New Device Create icon](./media/user-guide/usbx-events/image211.png)    | **Host Stack New Device Create** *(ux_host_stack_new_device_create)* |
| ![Host Stack New Endpoint Create icon](./media/user-guide/usbx-events/image212.png)    | **Host Stack New Endpoint Create** *(ux_host_stack_new_endpoint_create)* |
| ![Host Stack Root Hub Change Process icon](./media/user-guide/usbx-events/image213.png)    | **Host Stack Root Hub Change Process** *(ux_host_stack_rh_change_process)* |
| ![Host Stack Root Hub Device Extraction icon](./media/user-guide/usbx-events/image214.png)    | **Host Stack Root Hub Device Extraction** *(ux_host_stack_rh_device_extraction)* |
| ![Host Stack Root Hub Device Insertion icon](./media/user-guide/usbx-events/image215.png)    | **Host Stack Root Hub Device Insertion** *(ux_host_stack_rh_device_insertion)* |
| ![Host Stack Transfer Request icon](./media/user-guide/usbx-events/image216.png)    | **Host Stack Transfer Request** *(ux_host_stack_transfer_request)* |
| ![Host Stack Transfer Request Abort icon](./media/user-guide/usbx-events/image217.png)    | **Host Stack Transfer Request Abort** *(ux_host_stack_transfer_request_abort)* |
| ![U S B X Error icon](./media/user-guide/usbx-events/image218.png)    | **USBX Error** *(ux_error)* |

## Event Descriptions

The following pages describe the USBX Trace Events.

### Device Class Cdc Activate 

#### ux_device_class_cdc_activate

**Icon** ![Device Class C D C Activate icon](./media/user-guide/usbx-events/image1.png)

**Description**

This event represents a USBX Device Class Cdc Activate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Cdc Deactivate 

#### ux_device_class_cdc_deactivate

**Icon** ![Device Class C D C Deactivate icon](./media/user-guide/usbx-events/image2.png)

**Description**

This event represents a USBX Device Class Cdc Deactivate.

Information Fields 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Cdc Read 

#### ux_device_class_cdc_read

**Icon** ![Device Class C D C Read icon](./media/user-guide/usbx-events/image3.png)

**Description**

This event represents a USBX Device Class Cdc Read Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Device Class Cdc Write 

#### ux_device_class_cdc_write

**Icon** ![Device Class C D C Write icon](./media/user-guide/usbx-events/image4.png)

**Description**

This event represents a USBX Device Class Cdc Write Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Device Class Dpump Activate 

#### ux_device_class_dpump_activate

**Icon** ![Device Class Dpump Activate icon](./media/user-guide/usbx-events/image5.png)

**Description**

This event represents a USBX Device Class Dpump Activate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Dpump Deactivate 

#### ux_device_class_dpump_deactivate

**Icon** ![Device Class Dpump Deactivate icon](./media/user-guide/usbx-events/image6.png)

**Description**

This event represents a USBX Device Class Dpump Deactivate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Dpump Read 

#### ux_device_class_dpump_read

**Icon** ![Device Class Dpump Read icon](./media/user-guide/usbx-events/image7.png)

**Description**

This event represents a USBX Device Class Dpump Read Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Buffer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Device Class Dpump Write 

#### ux_device_class_dpump_write

**Icon** ![Device Class Dpump Write icon](./media/user-guide/usbx-events/image8.png)

**Description**

This event represents a USBX Device Class Dpump Write Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Device Class Hid Activate 

#### ux_device_class_hid_activate

**Icon** ![Device Class Hid Activate icon](./media/user-guide/usbx-events/image9.png)

**Description**

This event represents a USBX Device Class Hid Activate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Hid Deactivate 

#### ux_device_class_hid_deactivate

**Icon** ![Device Class Hid Deactivate icon](./media/user-guide/usbx-events/image10.png)

**Description**

This event represents a USBX Device Class Hid Deactivate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Hid Descriptor Send 

#### ux_device_class_hid_descriptor_send

**Icon** ![Device Class Hid Descriptor Send icon](./media/user-guide/usbx-events/image11.png)

**Description**

This event represents a USBX Device Class Hid Descriptor Send Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Descriptor type.
- Info Field 3: Request index.
- Info Field 4: Not used.

### Device Class Hid Event Get 

#### ux_device_class_hid_event_get

**Icon** ![Device Class Hid Event Get icon](./media/user-guide/usbx-events/image12.png)

**Description**

This event represents a USBX Device Class Hid Event Get Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Hid Event Set 

#### ux_device_class_hid_event_set

**Icon** ![Device Class Hid Event Set icon](./media/user-guide/usbx-events/image13.png)

**Description**

This event represents a USBX Device Class Hid Event Set.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Hid Report Get 

#### ux_device_class_hid_report_get

**Icon** ![Device Class Hid Report Get icon](./media/user-guide/usbx-events/image14.png)

**Description**

This event represents a USBX Device Class Hid Report Get Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Descriptor type.
- Info Field 3: Request index.
- Info Field 4: Not used.

### Device Class Hid Report Set 

#### ux_device_class_hid_report_set

**Icon** ![Device Class Hid Report Set icon](./media/user-guide/usbx-events/image15.png)

**Description**

This event represents a USBX Device Class Hid Report Set Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Descriptor type.
- Info Field 3: Request index.
- Info Field 4: Not used.

### Device Class Pima Activate

#### ux_device_class_pima_activate

**Icon** ![Device Class Pima Activate icon](./media/user-guide/usbx-events/image16.png)

**Description**

This event represents a USBX Device Class Pima Activate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Deactivate 

#### ux_device_class_pima_deactivate

**Icon** ![Device Class Pima Deactivate icon](./media/user-guide/usbx-events/image17.png)

**Description**

This event represents a USBX Device Class Pima Deactivate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Device Info Send 

#### ux_device_class_pima_device_info_send

**Icon** ![Device Class Pima Device Info Send icon](./media/user-guide/usbx-events/image18.png)

**Description**

This event represents a USBX Device Class Pima Device Info Send Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.

### Device Class Pima Event Get 

#### ux_device_class_pima_event_get

**Icon** ![Device Class Pima Event Get icon](./media/user-guide/usbx-events/image19.png)

**Description**

This event represents a USBX Device Class Pima Event Get Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Pima event.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Event Set 

#### ux_device_class_pima_event_set

**Icon** ![Device Class Pima Event Set icon](./media/user-guide/usbx-events/image20.png)

**Description**

This event represents a USBX Device Class Pima Event Set Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Pima event.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Add 

#### ux_device_class_pima_object_add

**Icon** ![Device Class Pima Object Add icon](./media/user-guide/usbx-events/image21.png)

**Description**

This event represents a USBX Device Class Pima Object Add Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Data Get 

#### ux_device_class_pima_object_data_get

**Icon** ![Device Class Pima Object Data Get icon](./media/user-guide/usbx-events/image22.png)

**Description**

This event represents a USBX Device Class Pima Object Data Get Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Data Send 

#### ux_device_class_pima_object_data_send

**Icon** ![Device Class Pima Object Data Send icon](./media/user-guide/usbx-events/image23.png)

**Description**

This event represents a USBX Device Class Pima Object Data Send Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Delete 

#### ux_device_class_pima_object_delete

**Icon** ![Device Class Pima Object Delete icon](./media/user-guide/usbx-events/image24.png)

**Description**

This event represents a USBX Device Class Pima Object Delete Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Handles Send 

#### ux_device_class_pima_object_handles_send

**Icon** ![Device Class Pima Object Handles Send icon](./media/user-guide/usbx-events/image25.png)

**Description**

This event represents a USBX Device Class Pima Object Handles Send Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Storage ID.
- Info Field 3: Object format code.
- Info Field 4: Object association.

### Device Class Pima Object Info Get 

#### ux_device_class_pima_object_info_send

**Icon** ![Device Class Pima Object Info Get icon](./media/user-guide/usbx-events/image26.png)

**Description**

This event represents a USBX Device Class Pima Object Info Get Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Info Send 

#### ux_device_class_pima_object_info_send

**Icon** ![Device Class Pima Object Info Send icon](./media/user-guide/usbx-events/image27.png)

**Description**

This event represents a USBX Device Class Pima Object Info Send Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Objects Number Send 

#### ux_device_class_pima_object_number_send

**Icon** ![Device Class Pima Objects Number Send icon](./media/user-guide/usbx-events/image28.png)

**Description**

This event represents a USBX Device Class Pima Object Number Send event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Storage ID.
- Info Field 3: Object format code.
- Info Field 4: Object associate.

### Device Class Pima Partial Object Data Get

#### ux_device_class_pima_partial_object_data_get

**Icon** ![Device Class Pima Partial Object Data Get icon](./media/user-guide/usbx-events/image29.png)

**Description**

This event represents a USBX Device Class Pima Partial Object Data Get Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Offset requested.
- Info Field 4: Length requested.

### Device Class Pima Response Send 

#### ux_device_class_pima_response_send

**Icon** ![Device Class Pima Response Send icon](./media/user-guide/usbx-events/image30.png)

**Description**

This event represents a USBX Device Class Pima Response Send Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Response code.
- Info Field 3: Number parameter.
- Info Field 4: Pima parameter 1.

### Device Class Pima Storage Id Send 

#### ux_device_class_pima_storage_id_send

**Icon** ![Device Class Pima Storage Id Send icon](./media/user-guide/usbx-events/image31.png)

**Description**

This event represents a USBX Device Class Pima Storage Id Send Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Storage Info Send 

#### ux_device_class_pima_storage_info_send

**Icon** ![Device Class Pima Storage Info Send icon](./media/user-guide/usbx-events/image32.png)

**Description**

This event represents a USBX Device Class Pima Storage Info Send Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Activate 

#### ux_device_class_rndis_activate

**Icon** ![Device Class Rndis Activate icon](./media/user-guide/usbx-events/image33.png)

**Description**

This event represents a USBX Device Class Rndis Activate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Deactivate 

#### ux_device_class_rndis_deactivate

**Icon** ![Device Class Rndis Deactivate icon](./media/user-guide/usbx-events/image34.png)

**Description**

This event represents a USBX Device Class Rndis Deactivate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Message Keep Alive 

#### ux_device_class_rndis_msg_keep_alive

**Icon** ![Device Class Rndis Message Keep Alive icon](./media/user-guide/usbx-events/image35.png)

**Description**

This event represents a USBX Device Class Rndis Message Keep Alive Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Message Query 

#### ux_device_class_rndis_msg_keep_query

**Icon** ![Device Class Rndis Message Query icon](./media/user-guide/usbx-events/image36.png)

**Description**

This event represents a USBX Device Class Rndis Message Query Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Rndis OID.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Message Reset 

#### ux_device_class_rndis_msg_reset

**Icon** ![Device Class Rndis Message Reset icon](./media/user-guide/usbx-events/image37.png)

**Description**

This event represents a USBX Device Class Rndis Message Reset Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.

### Device Class Rndis Message Set 

#### ux_device_class_rndis_msg_set

**Icon** ![Device Class Rndis Message Set icon](./media/user-guide/usbx-events/image38.png)

**Description**

This event represents a USBX Device Class Rndis Message Set Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Rndis OID.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Packet Receive 

#### ux_device_class_rndis_packet_receive

**Icon** ![Device Class Rndis Packet Receive icon](./media/user-guide/usbx-events/image39.png)

**Description**

This event represents a USBX Device Class Rndis Packet Receive Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Packet Transmit 

#### ux_device_class_rndis_packet_transmit

**Icon** ![Device Class Rndis Packet Transmit icon](./media/user-guide/usbx-events/image40.png)

**Description**

This event represents a USBX Device Class Rndis Packet Transmit Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Activate 

#### ux_device_class_storage_activate

**Icon** ![Device Class Storage Activate icon](./media/user-guide/usbx-events/image41.png)

**Description**

This event represents a USBX Device Class Storage Activate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Deactivate 

#### ux_device_class_storage_deactivate

**Icon** ![Device Class Storage Deactivate icon](./media/user-guide/usbx-events/image42.png)

**Description**

This event represents a USBX Device Class Storage Deactivate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Format 

#### ux_device_class_storage_format

**Icon** ![Device Class Storage Format icon](./media/user-guide/usbx-events/image43.png)

**Description**

This event represents a USBX Device Class Storage Format Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Inquiry 

#### ux_device_class_storage_inquiry

**Icon** ![Device Class Storage Inquiry icon](./media/user-guide/usbx-events/image44.png)

**Description**

This event represents a USBX Device Class Storage Inquiry Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Mode Select

#### ux_device_class_storage_mode_select

**Icon** ![Device Class Storage Mode Select icon](./media/user-guide/usbx-events/image45.png)

**Description**

This event represents a USBX Device Class Storage Mode Select Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Mode Sense 

#### ux_device_class_storage_mode_sense

**Icon** ![Device Class Storage Mode Sense icon](./media/user-guide/usbx-events/image46.png)

**Description**

This event represents a USBX Device Class Storage Mode Sense Event.

Information Fields 

- nfo Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Prevent Allow Media Removal 

#### ux_device_class_storage_prevent_allow_media_removal

**Icon** ![Device Class Storage Prevent Allow Media Removal icon](./media/user-guide/usbx-events/image47.png)

**Description**

This event represents a USBX Device Class Storage Prevent Allow Media Removal Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Read 

#### ux_device_class_storage_read

**Icon** ![Device Class Storage Read icon](./media/user-guide/usbx-events/image48.png)

**Description**

This event represents a USBX Device Class Storage Read Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Sector.
- Info Field 4: Number sectors.

### Device Class Storage Read Capacity 

#### ux_device_class_storage_read_capacity

**Icon** ![Device Class Storage Read Capacity icon](./media/user-guide/usbx-events/image49.png)

**Description**

This event represents a USBX Device Class Storage Read Capacity Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Read Format Capacity 

#### ux_device_class_storage_read_format_capacity

**Icon** ![Device Class Storage Read Format Capacity icon](./media/user-guide/usbx-events/image50.png)

**Description**

This event represents a USBX Device Class Storage Read Format Capacity Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Read TOC 

#### ux_device_class_storage_read_toc

**Icon** ![Device Class Storage Read TOC icon](./media/user-guide/usbx-events/image51.png)

**Description**

This event represents a USBX Device Class Storage Read TOC Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Request Sense 

#### ux_device_class_storage_request_sense

**Icon** ![Device Class Storage Request Sense icon](./media/user-guide/usbx-events/image52.png)

**Description**

This event represents a USBX Device Class Storage Request Sense Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Sense key.
- Info Field 4: Code.

### Device Class Storage Start Stop 

#### ux_device_class_storage_start_stop

**Icon** ![Device Class Storage Start Stop icon](./media/user-guide/usbx-events/image53.png)

**Description**

This event represents a USBX Device Class Storage Start Stop Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Test Ready 

#### ux_device_class_storage_test_ready

**Icon** ![Device Class Storage Test Ready icon](./media/user-guide/usbx-events/image54.png)

**Description**

This event represents a USBX Device Class Storage Test Ready Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Verify 

#### ux_device_class_storage_verify

**Icon** ![Device Class Storage Verify icon](./media/user-guide/usbx-events/image55.png)

**Description**

This event represents a USBX Device Class Storage Verify Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Write 

#### ux_device_class_storage_write

**Icon** ![Device Class Storage Write icon](./media/user-guide/usbx-events/image56.png)

**Description**

This event represents a USBX Device Class Storage Write Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Sector.
- Info Field 4: Number sectors.

### Device Stack Alternate Setting Get 

#### ux_device_class_alternate_setting_get

**Icon** ![Device Stack Alternate Setting Get icon](./media/user-guide/usbx-events/image57.png)

**Description**

This event represents a USBX Device Stack Alternate Setting Get Event.

**Information Fields**

- Info Field 1: Interface value.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Alternate Setting Set 

#### ux_device_class_alternate_setting_set

**Icon** ![Device Stack Alternate Setting Set icon](./media/user-guide/usbx-events/image58.png)

**Description**

This event represents a USBX Device Stack Alternate Setting Set Event.

**Information Fields**
- Info Field 1: Interface value.
- Info Field 2: Alternate setting value.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Class Register 

#### ux_device_stack_class_register

**Icon** ![Device Stack Class Register icon](./media/user-guide/usbx-events/image59.png)

**Description**

This event represents a USBX Device Stack Class Register Event.

**Information Fields**

- Info Field 1: Class name.
- Info Field 2: Interface number.
- Info Field 3: Parameter.
- Info Field 4: Not used.

### Device Stack Clear Feature 

#### ux_device_stack_clear_feature

**Icon** ![Device Stack Clear Feature icon](./media/user-guide/usbx-events/image60.png)

**Description**

This event represents a USBX Device Stack Clear Feature Event.

**Information Fields**

- Info Field 1: Request type.
- Info Field 2: Request value. Info Field 3: Request index.
- Info Field 4: Not used.

### Device Stack Configuration Get 

#### ux_device_stack_configuration_get t

**Icon** ![Device Stack Configuration Get icon](./media/user-guide/usbx-events/image61.png)

**Description**

This event represents a USBX Device Stack Configuration Get Event.

**Information Fields** 

- Info Field 1: Configuration value.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Configuration Set 

#### ux_device_stack_configuration_set

**Icon** ![Device Stack Configuration Set icon](./media/user-guide/usbx-events/image62.png)

**Description**

This event represents a USBX Device Stack Configuration Set Event.

**Information Fields** 

- Info Field 1: Configuration value.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Connect 

#### ux_device_stack_connect

**Icon** ![Device Stack Connect icon](./media/user-guide/usbx-events/image63.png)

**Description**

This event represents a USBX Device Stack Descriptor Send Event.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Descriptor Send 

#### ux_device_stack_descriptor_send

**Icon** ![Device Stack Descriptor Send icon](./media/user-guide/usbx-events/image64.png)

**Description**

This event represents a USBX Device Stack Descriptor Send Event.

**Information Fields**

- Info Field 1: Descriptor type.
- Info Field 2: Request index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Disconnect 

#### ux_device_stack_disconnect

**Icon** ![Device Stack Disconnect icon](./media/user-guide/usbx-events/image65.png)

**Description**

This event represents a USBX Device Stack Disconnect Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Endpoint Stall 

#### ux_device_stack_endpoint_stall

**Icon** ![Device Stack Endpoint Stall icon](./media/user-guide/usbx-events/image66.png)

**Description**

This event represents a USBX Device Stack Endpoint Stall Event.

**Information Fields** 

- Info Field 1: Endpoint.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Get Status 

#### ux_device_stack_get_status

**Icon** ![Device Stack Get Status icon](./media/user-guide/usbx-events/image67.png)

**Description**

This event represents a USBX Device Stack Get Status Event.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Host Wakeup 

#### ux_device_stack_host_wakeup

**Icon** ![Device Stack Host Wakeup icon](./media/user-guide/usbx-events/image68.png)

**Description**

This event represents a USBX Device Stack Host Wakeup Event.

**Information Fields** 

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Initialize 

#### ux_device_stack_initialize

**Icon** ![Device Stack Initialize icon](./media/user-guide/usbx-events/image69.png)

**Description**

This event represents a USBX Device Stack Initialize Event.

**Information Fields** 

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Interface Delete 

#### ux_device_stack_interface_delete

**Icon** ![Device Stack Interface Delete icon](./media/user-guide/usbx-events/image70.png)

**Description**

This event represents a USBX Device Stack Interface Delete Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Interface Get 

#### ux_device_stack_interface_get

**Icon** ![Device Stack Interface Get icon](./media/user-guide/usbx-events/image71.png)

**Description**

This event represents a USBX Device Stack Interface Get Event.

**Information Fields** 

- Info Field 1: Interface value.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Interface Set 

#### ux_device_stack_interface_set

**Icon** ![Device Stack Interface Set icon](./media/user-guide/usbx-events/image72.png)

**Description**

This event represents a USBX Device Stack Interface Set Event.

**Information Fields** 

- Info Field 1: Request value. Info Field 2: Request index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Set Feature 

#### ux_device_stack_set_feature

**Icon** ![Device Stack Set Feature icon](./media/user-guide/usbx-events/image73.png)

**Description**

This event represents a USBX Device Stack Set Feature Event.

**Information Fields** 

- Info Field 1: Request value. Info Field 2: Request index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Transfer Abort 

#### ux_device_stack_transfer_abort

**Icon** ![Device Stack Transfer Abort icon](./media/user-guide/usbx-events/image74.png)

**Description**

This event represents a USBX Device Stack Transfer Abort Event.

**Information Fields** 

- Info Field 1: Transfer request.
- Info Field 2: Completion code.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Transfer All Request Abort 

#### ux_device_stack_transfer_all_request_abort

**Icon** ![Device Stack Transfer All Request Abort icon](./media/user-guide/usbx-events/image75.png)

**Description**

This event represents a USBX Device Stack Transfer All Request Abort Event.

**Information Fields** 

- Info Field 1: Endpoint.
- Info Field 2: Completion code.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Transfer Request 

#### ux_device_stack_transfer_request

**Icon** ![Device Stack Transfer Request icon](./media/user-guide/usbx-events/image76.png)

**Description**

This event represents a USBX Device Stack Transfer Request Event.

**Information Fields**

- Info Field 1: Transfer request.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Asix Activate 

#### ux_host_class_asix_activate

**Icon** ![Host Class Asix Activate icon](./media/user-guide/usbx-events/image77.png)

**Description**

This event represents a USBX Host Class Asix Activate.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Asix Deactivate 

#### ux_host_class_asix_deactivate

**Icon** ![Host Class Asix Deactivate icon](./media/user-guide/usbx-events/image78.png)

**Description**

This event represents a USBX Host Class Asix Deactivate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Asix Interrupt Notification 

#### ux_host_class_asix_interrupt_notification

**Icon** ![Host Class Asix Interrupt Notification icon](./media/user-guide/usbx-events/image79.png)

**Description**

This event represents a USBX Host Class Asix Interrupt Notification Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Asix Read 

#### ux_host_class_asix_read

**Icon** ![Host Class Asix Read icon](./media/user-guide/usbx-events/image80.png)

**Description**

This event represents a USBX Host Class Asix Read Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Asix Write 

#### ux_host_class_asix_write

**Icon** ![Host Class Asix Write icon](./media/user-guide/usbx-events/image81.png)

**Description**

This event represents a USBX Host Class Asix Write Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Audio Activate 

#### ux_host_class_audio_activate

**Icon** ![Host Class Audio Activate icon](./media/user-guide/usbx-events/image82.png)

**Description**

This event represents a USBX Host Class Audio Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Control Value Get 

#### ux_host_class_audio_control_value_get

**Icon** ![Host Class Audio Control Value Get icon](./media/user-guide/usbx-events/image83.png)

**Description**

This event represents a USBX Host Class Audio Control Value Get Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Control Value Set 

#### ux_host_class_audio_control_value_set

**Icon** ![Host Class Audio Control Value Set icon](./media/user-guide/usbx-events/image84.png)

**Description**

This event represents an internal NetX Duo I/O driver deferred processing event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Audio control.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Deactivate 

#### ux_host_class_audio_deactivate

**Icon** ![Host Class Audio Deactivate icon](./media/user-guide/usbx-events/image85.png)

**Description**

This event represents a USBX Host Class Audio Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Read 

#### ux_host_class_audio_read

**Icon** ![Host Class Audio Read icon](./media/user-guide/usbx-events/image86.png)

**Description**

This event represents a USBX Host Class Audio Read Event.

- Information Fields 
- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Audio Streaming Sampling Get 

#### ux_host_class_audio_streaming_sampling_get

**Icon** ![Host Class Audio Streaming Sampling Get icon](./media/user-guide/usbx-events/image87.png)

**Description**

This event represents a USBX Host Class Audio Streaming Sampling Get Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Streaming Sampling Set 

#### ux_host_class_audio_streaming_sampling_set

**Icon** ![Host Class Audio Streaming Sampling Set icon](./media/user-guide/usbx-events/image88.png)

**Description**

This event represents a USBX Host Class Audio Streaming Sampling Set Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Audio Sampling.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Write 

#### ux_host_class_audio_write

**Icon** ![Host Class Audio Write icon](./media/user-guide/usbx-events/image89.png)

**Description**

This event represents a USBX Host Class Audio Write Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Cdc Acm Activate 

#### ux_host_class_cdc_acm_activate

**Icon** ![Host Class C D C A C M Activate icon](./media/user-guide/usbx-events/image90.png)

**Description**

This event represents a USBX Host Class Cdc Acm Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Deactivate 

#### ux_host_class_cdc_acm_deactivate

**Icon** ![Host Class C D C A C M Deactivate icon](./media/user-guide/usbx-events/image91.png)

**Description**

This event represents a USBX Host Class Cdc Acm Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Abort In Pipe 

#### ux_host_class_cdc_acm_ioctl_abort_in_pipe

**Icon** ![Host Class C D C A C M I O C T L Abort In Pipe icon](./media/user-guide/usbx-events/image92.png)

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Abort In Pipe Event.

Information Fields 

- Info Field 1: Class instance.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Abort Out Pipe 

#### ux_host_class_cdc_acm_ioctl_abort_out_pipe

**Icon** ![[Host Class C D C A C M I O C T L Abort Out Pipe icon](./media/user-guide/usbx-events/image93.png)

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Abort Out Pipe Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Get Device Status 

#### ux_host_class_cdc_acm_ioctl_get_device_status

**Icon** ![Host Class C D C A C M I O C T L Get Device Status icon](./media/user-guide/usbx-events/image94.png)

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Get Device Status Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Device status.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Get Line Coding 

#### ux_host_class_cdc_acm_ioctl_get_line_coding

**Icon** ![Host Class C D C A C M I O C T L Get Line Coding icon](./media/user-guide/usbx-events/image95.png)

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Get Line Coding Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Notification Callback

#### ux_host_class_cdc_acm_ioctl_notification_callback

**Icon** ![Host Class C D C A C M I O C T L Notification Callback icon](./media/user-guide/usbx-events/image96.png)

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Notification Callback Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Send Break 

#### ux_host_class_cdc_acm_ioctl_send_break

**Icon** ![Host Class C D C A C M I O C T L Send Break icon](./media/user-guide/usbx-events/image97.png)

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Send Break Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Set Line Coding 

#### ux_host_class_cdc_acm_ioctl_set_line_coding

**Icon** ![The Host Class C D C A C M I O C T L Set Line Coding icon](./media/user-guide/usbx-events/image97.png)
 icon](./media/user-guide/usbx-events/image98.png)

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Set Line Coding Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Set Line State 

#### ux_host_class_cdc_acm_ioctl_set_line_state

**Icon** ![Host Class C D C A C M I O C T L Set Line State icon](./media/user-guide/usbx-events/image99.png)

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Set Line State Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Read 

#### ux_host_class_cdc_acm_read

**Icon** ![Host Class C D C A C M Read icon](./media/user-guide/usbx-events/image100.png)

**Description**

This event represents a USBX Host Class Cdc Acm Read Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested Length.
- Info Field 4: Not used.

### Host Class Cdc Acm Reception Start 

#### ux_host_class_cdc_acm_reception_start

**Icon** ![Host Class C D C A C M Reception Start icon](./media/user-guide/usbx-events/image101.png)

**Description**

This event represents a USBX Host Class Cdc Acm Reception Start Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Reception Stop 

#### ux_host_class_cdc_acm_reception_stop

**Icon** ![Host Class C D C A C M Reception Stop icon](./media/user-guide/usbx-events/image102.png)

**Description**

This event represents a USBX Host Class Cdc Acm Reception Stop Event.

**Information Fields**

- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Write 

#### ux_host_class_acm_write

**Icon** ![Host Class C D C A C M Write icon](./media/user-guide/usbx-events/image103.png)

**Description**

This event represents a USBX Host Class Cdc Acm Write Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested Length.
- Info Field 4: Not used.

### Host Class Dpump Activate 

#### ux_host_class_dpump_activate

**Icon** ![Host Class Dpump Activate icon](./media/user-guide/usbx-events/image104.png)

**Description**

This event represents a USBX Host Class Dpump Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Dpump Deactivate 

#### ux_host_class_dpump_deactivate

**Icon** ![Host Class Dpump Deactivate icon](./media/user-guide/usbx-events/image105.png)

**Description**

This event represents a USBX Host Class Dpump Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Dpump Read 

#### ux_host_class_dpump_read

**Icon** ![Host Class Dpump Read icon](./media/user-guide/usbx-events/image106.png)

**Description**

This event represents a USBX Host Class Dpump Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Dpump Write 

#### ux_host_class_dpump_write

**Icon** ![Host Class Dpump Write icon](./media/user-guide/usbx-events/image107.png)

**Description**

This event represents a USBX Host Class Dpump Write Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Hid Activate 

#### ux_host_class_hid_activate

**Icon** ![Host Class Hid Activate icon](./media/user-guide/usbx-events/image108.png)

**Description**

This event represents a USBX Host Class Hid Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Client Register 

#### ux_host_class_hid_client_register

**Icon** ![Host Class Hid Client Register icon](./media/user-guide/usbx-events/image109.png)

**Description**

This event represents a USBX Host Class Hid Client Register Event.

**Information Fields** 

- Info Field 1: Hid client name.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Deactivate 

#### ux_host_class_hid_deactivate

**Icon** ![Host Class Hid Deactivate icon](./media/user-guide/usbx-events/image110.png)

**Description**

This event represents a USBX Host Class Hid Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Idle Get 

#### ux_host_class_hid_idle_get

**Icon** ![Host Class Hid Idle Get icon](./media/user-guide/usbx-events/image111.png)

**Description**

This event represents a USBX Host Class Hid Idle Get Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Idle Set 

#### ux_host_class_hid_idle_set

**Icon** ![Host Class Hid Idle Set icon](./media/user-guide/usbx-events/image112.png)

**Description**

This event represents a USBX Host Class Hid Idle Set Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Keyboard Activate 

#### ux_host_class_hid_keyboard_activate

**Icon** ![Host Class Hid Keyboard Activate icon](./media/user-guide/usbx-events/image113.png)

**Description**

This event represents a USBX Host Class Hid Keyboard Activate Event.

**Information Fields**
<p>Info Field 1: Class instance.
<p>Info Field 2: Hid client instance.
<p>Info Field 3: Not used.
<p>Info Field 4: Not used.

### Host Class Hid Keyboard Deactivate 

#### ux_host_class_hid_keyboard_deactivate

**Icon** ![Host Class Hid Keyboard Deactivate icon](./media/user-guide/usbx-events/image114.png)

**Description**

This event represents a USBX Host Class Hid Keyboard Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Mouse Activate 

#### ux_host_class_hid_mouse_activate

**Icon** ![Host Class Hid Mouse Activate icon](./media/user-guide/usbx-events/image115.png)

**Description**

This event represents a USBX Host Class Hid Mouse Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Mouse Deactivate 

#### ux_host_class_hid_mouse_deactivate

**Icon** ![Host Class Hid Mouse Deactivate icon](./media/user-guide/usbx-events/image116.png)

**Description**

- This event represents a USBX Host Class Hid Mouse Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Remote Control Activate 

#### ux_host_class_hid_remote_control_activate

**Icon** ![Host Class Hid Remote Control Activate icon](./media/user-guide/usbx-events/image117.png)

**Description**

This event represents a USBX Host Class Hid Remote Control Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Remote Control Deactivate 

#### ux_host_class_hid_remote_control_deactivate

**Icon** ![Host Class Hid Remote Control Deactivate icon](./media/user-guide/usbx-events/image118.png)

**Description**

This event represents a USBX Host Class Hid Remote Control Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Report Get 

#### ux_host_class_hid_report_get

**Icon** ![Host Class Hid Report Get icon](./media/user-guide/usbx-events/image119.png)

**Description**

This event represents a USBX Host Class Hid Report Get.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Client report.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Report Set 

#### ux_host_class_hid_report_set

**Icon** ![Host Class Hid Report Set icon](./media/user-guide/usbx-events/image120.png)

**Description**
This event represents a USBX Host Class Hid Report Set.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Client report.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hub Activate 

#### ux_host_class_hub_activate

**Icon** ![Host Class Hub Activate icon](./media/user-guide/usbx-events/image121.png)

**Description**

This event represents a USBX Host Class Hub Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hub Change Detect 

#### ux_host_class_hub_change_detect

**Icon** ![Host Class Hub Change Detect icon](./media/user-guide/usbx-events/image122.png)

**Description**

This event represents a USBX Host Class Hub Change Detect Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hub Deactivate 

#### ux_host_class_hub_deactivate

**Icon** ![Host Class Hub Deactivate icon](./media/user-guide/usbx-events/image123.png)

**Description**

This event represents a USBX Host Class Hub Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hub Port Change Connection Process 

#### ux_host_class_hub_change_connection_process

**Icon** ![Host Class Hub Port Change Connection Process icon](./media/user-guide/usbx-events/image124.png)

**Description**

This event represents a USBX Host Class Hub Port Change Connection Process Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Hub Port Change Enable Process 

#### ux_host_class_hub_port_change_enable_process

**Icon** ![Host Class Hub Port Change Enable Process icon](./media/user-guide/usbx-events/image125.png)

**Description**

This event represents a USBX Host Class Hub Port Change Enable Process Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Hub Port Change Over Current Process 

#### ux_host_class_hub_port_change_over_current_process

**Icon** ![Host Class Hub Port Change Over Current Process icon](./media/user-guide/usbx-events/image126.png)

**Description**

This event represents allocating a packet via nx_packet_allocate.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Hub Port Change Reset Process 

#### ux_host_class_hub_port_change_reset_process

**Icon** ![Host Class Hub Port Change Reset Process icon](./media/user-guide/usbx-events/image127.png)

**Description**

This event represents a USBX Host Class Hub Port Change Reset Process Event.

**Information Fields**

- Info Field 1: Hub. Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Hub Port Change Suspend Process 

#### ux_host_class_hub_port_change_suspend_process

**Icon** ![Host Class Hub Port Change Suspend Process icon](./media/user-guide/usbx-events/image128.png)

**Description**

This event represents a USBX Host Class Hub Port Change Suspend Process Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Pima Activate 

#### ux_host_class_pima_activate

**Icon** ![Host Class Pima Activate icon](./media/user-guide/usbx-events/image129.png)

**Description**

This event represents a USBX Host Class Pima Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Deactivate 

#### ux_host_class_pima_deactivate

**Icon** ![Host Class Pima Deactivate icon](./media/user-guide/usbx-events/image130.png)

**Description**

This event represents a USBX Host Class Pima Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Device Info Get 

#### ux_host_class_pima_device_info_get

**Icon** ![Host Class Pima Device Info Get icon](./media/user-guide/usbx-events/image131.png)

**Description**

This event represents a USBX Host Class Pima Device Info Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Pima device.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Device Reset 

#### ux_host_class_pima_device_reset

**Icon** ![Host Class Pima Device Reset icon](./media/user-guide/usbx-events/image132.png)

**Description**

This event represents a USBX Host Class Pima Device Reset Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Notification 

#### ux_host_class_pima_notification

**Icon** ![Host Class Pima Notification icon](./media/user-guide/usbx-events/image133.png)

**Description**

This event represents a USBX Host Class Pima Notification Event.

**Information Fields**

- Info Field 1: Class instance. Info Field 2: Event code.
- Info Field 3: Transaction ID.
- Info Field 4: Parameter1.

### Host Class Pima Number Objects Get 

#### ux_host_class_pima_number_objects_get

**Icon** ![Host Class Pima Number Objects Get icon](./media/user-guide/usbx-events/image134.png)

**Description**

This event represents a USBX Host Class Pima Number Objects Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Close 

#### ux_host_class_pima_object_close

**Icon** ![Host Class Pima Object Close icon](./media/user-guide/usbx-events/image135.png)

**Description**

This event represents a USBX Host Class Pima Object Close Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Copy 

#### ux_host_class_pima_object_copy

**Icon** ![Host Class Pima Object Copy icon](./media/user-guide/usbx-events/image136.png)

**Description**

This event represents a USBX Host Class Pima Object Copy Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Delete 

#### ux_host_class_pima_object_delete

**Icon** ![Host Class Pima Object Delete icon](./media/user-guide/usbx-events/image137.png)

**Description**

This event represents a USBX Host Class Pima Object Delete Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Get 

#### ux_host_class_pima_object_get

**Icon** ![Host Class Pima Object Get icon](./media/user-guide/usbx-events/image138.png)

**Description**

This event represents getting RARP information via nx_rarp_info_get.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Object.
- Info Field 4: Not used.

### Host Class Pima Object Info Get 

#### ux_host_class_pima_object_info_get

**Icon** ![Host Class Pima Object Info Get icon](./media/user-guide/usbx-events/image139.png)

**Description**

This event represents a USBX Host Class Pima Object Info Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Object.
- Info Field 4: Not used.

### Host Class Pima Object Info Send 

#### ux_host_class_pima_object_info_send

**Icon** ![Host Class Pima Object Info Send icon](./media/user-guide/usbx-events/image140.png)

**Description**

This event represents a USBX Host Class Pima Object Info Send Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Move 

#### ux_host_class_pima_object_move

**Icon** ![Host Class Pima Object Move icon](./media/user-guide/usbx-events/image141.png)

**Description**

This event reprThis event represents a USBX Host Class Pima Object Move Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Object.
- Info Field 4: Not used.

### Host Class Pima Object Send 

#### ux_host_class_pima_object_move

**Icon** ![Host Class Pima Object Send icon](./media/user-guide/usbx-events/image142.png)

**Description**

This event represents a USBX Host Class Pima Object Send Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object.
- Info Field 3: Object buffer.
- Info Field 4: Object length.

### Host Class Pima Object Transfer Abort 

#### ux_host_class_pima_object_transfer_abort

**Icon** ![Host Class Pima Object Transfer Abort icon](./media/user-guide/usbx-events/image143.png)

**Description**

This event represents a USBX Host Class Pima Object Transfer Abort Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Object.
- Info Field 4: Not used.

### Host Class Pima Read 

#### ux_host_class_pima_read

**Icon** ![Host Class Pima Read icon](./media/user-guide/usbx-events/image144.png)

**Description**

This event represents a USBX Host Class Pima Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Data length.
- Info Field 4: Not used.

### Host Class Pima Request Cancel 

#### ux_host_class_pima_request_cancel

**Icon** ![Host Class Pima Request Cancel icon](./media/user-guide/usbx-events/image145.png)

**Description**

This event represents a USBX Host Class Pima Request Cancel Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Session Close 

#### ux_host_class_pima_session_close

**Icon** ![Host Class Pima Session Close icon](./media/user-guide/usbx-events/image146.png)

**Description**

This event represents a USBX Host Class Pima Session Close Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Pima session.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Session Open 

#### ux_host_class_pima_session_open

**Icon** ![Host Class Pima Session Open icon](./media/user-guide/usbx-events/image147.png)

**Description**
This event represents a USBX Host Class Pima Session Open Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Pima session.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Storage Ids Get 

#### ux_host_class_pima_session_ids_get

**Icon** ![Host Class Pima Storage Ids Get icon](./media/user-guide/usbx-events/image148.png)

**Description**

This event represents a USBX Host Class Pima Storage Ids Get Event.

**Information Fields**

- Info Field 1: Class instance.
- nfo Field 2: Storage ID array.
- Info Field 3: Storage ID length.
Info Field 4: Not used.

### Host Class Pima Storage Info Get 

#### ux_host_class_pima_storage_info_get

**Icon** ![Host Class Pima Storage Info Get icon](./media/user-guide/usbx-events/image149.png)

**Description**

This event represents a USBX Host Class Pima Storage Info Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Storage ID.
- Info Field 3: Storage.
- Info Field 4: Not used.

### Host Class Pima Thumb Get 

#### ux_host_class_pima_thumb_get

**Icon** ![Host Class Pima Thumb Get icon](./media/user-guide/usbx-events/image150.png)

**Description**

This event represents unaccepting a TCP server connection via nx_tcp_server_socket_unaccept.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Write 

#### ux_host_class_pima_thumb_get

**Icon** ![Host Class Pima Write icon](./media/user-guide/usbx-events/image151.png)

**Description**

This event represents a USBX Host Class Pima Write.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Data pointer.
- Info Field 3: Data length.
- Info Field 4: Not used.

### Host Class Printer Activate 

#### ux_host_class_printer_activate

**Icon** ![Host Class Printer Activate icon](./media/user-guide/usbx-events/image152.png)

**Description**

This event represents a USBX Host Class Printer Activate Event.

**Information Fields**

- Info Field 1: Pointer to IP instance.
- Info Field 2: Pointer to socket.
- Info Field 3: Type of service.
- Info Field 4: Receive window size.

### Host Class Printer Deactivate 

#### ux_host_class_printer_deactivate

**Icon** ![Host Class Printer Deactivate icon](./media/user-guide/usbx-events/image153.png)

**Description**

This event represents a USBX Host Class Printer Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Printer Name Get 

#### ux_host_class_printer_name_get

**Icon** ![Host Class Printer Name Get icon](./media/user-guide/usbx-events/image154.png)

**Description**

This event represents a USBX Host Class Printer Name Get Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Printer Read 

#### ux_host_class_printer_read

**Icon** ![Host Class Printer Read icon](./media/user-guide/usbx-events/image155.png)

**Description**

This event represents a USBX Host Class Printer Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Printer Soft Reset 

#### ux_host_class_printer_soft_reset

**Icon** ![Host Class Printer Soft Reset icon](./media/user-guide/usbx-events/image156.png)

**Description**

This event represents a USBX Host Class Printer Soft Reset Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Printer Status Get 

#### ux_host_class_printer_status_get

**Icon** ![Host Class Printer Status Get icon](./media/user-guide/usbx-events/image157.png)

**Description**

This event represents a USBX Host Class Printer Status Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Printer status.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Printer Write 

#### ux_host_class_printer_write

**Icon** ![Host Class Printer Write icon](./media/user-guide/usbx-events/image158.png)

**Description**

This event represents a USBX Host Class Printer Write.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Prolific Activate 

#### ux_host_class_prolific_activate 

**Icon** ![Host Class Prolific Activate icon](./media/user-guide/usbx-events/image159.png)

**Description**

This event represents a USBX Host Class Prolific Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Deactivate 

#### ux_host_class_prolific_deactivate 

**Icon** ![Host Class Prolific Deactivate icon](./media/user-guide/usbx-events/image160.png)

**Description**

This event represents a USBX Host Class Prolific Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Abort In Pipe 

#### ux_host_class_prolific_ioctl_abort_in_pipe

**Icon** ![Host Class Prolific I O C T L Abort In Pipe icon](./media/user-guide/usbx-events/image161.png)

**Description**

This event represents a USBX Host Class Prolific Ioctl Abort In Pipe Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Abort Out Pipe 

#### ux_host_class_prolific_ioctl_abort_out_pipe

**Icon** ![Host Class Prolific I O C T L Abort Out Pipe icon](./media/user-guide/usbx-events/image162.png)

**Description**

This event represents a USBX Host Class Prolific Ioctl Abort Out Pipe Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Get Device Status 

#### ux_host_class_prolific_ioctl_get_device_status

**Icon** ![Host Class Prolific I O C T L Get Device Status icon](./media/user-guide/usbx-events/image163.png)

**Description**

This event represents a USBX Host Class Prolific Ioctl Get Device Status Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Device status.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Get Line Coding 

#### ux_host_class_prolific_ioctl_get_line_coding

**Icon** ![Host Class Prolific I O C T L Get Line Coding icon](./media/user-guide/usbx-events/image164.png)

**Description**

This event represents a USBX Host Class Prolific Ioctl Get Line Coding Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Purge 

#### ux_host_class_prolific_ioctl_purge

**Icon** ![Host Class Prolific Ioctl Purge icon](./media/user-guide/usbx-events/image165.png)

**Description**

This event represents a USBX Host Class Prolific Ioctl Purge Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Report Device 

#### ux_host_class_prolific_ioctl_report_device

**Icon** ![Host Class Prolific I O C T L Report Device icon](./media/user-guide/usbx-events/image166.png)

**Description**

This event represents a USBX Host Class Prolific Ioctl Report Device Status Change Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Send Break 

#### ux_host_class_prolific_ioctl_send_break

**Icon** ![Host Class Prolific I O C T L Send Break icon](./media/user-guide/usbx-events/image167.png)

**Description**

This event represents a USBX Host Class Prolific Ioctl Send Break Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Set Line Coding 

#### ux_host_class_prolific_ioctl_set_line_coding

**Icon** ![Host Class Prolific I O C T L Set Line Coding icon](./media/user-guide/usbx-events/image168.png)

**Description**

This event represents a USBX Host Class Prolific Ioctl Set Line Coding Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Set Line State 

#### ux_host_class_prolific_ioctl_set_line_state

**Icon** ![Host Class Prolific I O C T L Set Line State icon](./media/user-guide/usbx-events/image169.png)

**Description**

This event represents a USBX Host Class Prolific Ioctl Set Line State Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Read 

#### ux_host_class_prolific_read

**Icon** ![Host Class Prolific Read icon](./media/user-guide/usbx-events/image170.png)

**Description**

This event represents a USBX Host Class Prolific Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Prolific Reception Start 

#### ux_host_class_prolific_reception_start

**Icon** ![Host Class Prolific Reception Start icon](./media/user-guide/usbx-events/image171.png)

**Description**

This event represents a USBX Host Class Prolific Reception Start Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Reception Stop 

#### ux_host_class_prolific_reception_stop

**Icon** ![Host Class Prolific Reception Stop icon](./media/user-guide/usbx-events/image172.png)

**Description**

This event represents a USBX Host Class Prolific Reception Stop Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Write 

#### ux_host_class_prolific_write

**Icon** ![Host Class Prolific Write icon](./media/user-guide/usbx-events/image173.png)

**Description**

This event represents a USBX Host Class Prolific Write Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Storage Activate 

#### ux_host_class_storage_activate

**Icon** ![Host Class Storage Activate icon](./media/user-guide/usbx-events/image174.png)

**Description**

This event represents a USBX Host Class Storage Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Deactivate 

#### ux_host_class_storage_deactivate

**Icon** ![Host Class Storage Deactivate icon](./media/user-guide/usbx-events/image175.png)

**Description**

This event represents a USBX Host Class Storage Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Media Capacity Get 

#### ux_host_class_storage_media_capacity_get

**Icon** ![Host Class Storage Media Capacity Get icon](./media/user-guide/usbx-events/image176.png)

**Description**

This event represents a USBX Host Class Storage Media Capacity Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Media Format Capacity Get

#### ux_host_class_storage_media_format_capacity_get

**Icon** ![Host Class Storage Media Format Capacity Get icon](./media/user-guide/usbx-events/image177.png)

**Description**

This event represents a USBX Host Class Storage Media Format Capacity Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

#### Host Class Storage Media Mount 

#### ux_host_class_storage_media_mount

**Icon** ![Host Class Storage Media Mount icon](./media/user-guide/usbx-events/image178.png)

**Description**

This event represents a USBX Host Class Storage Media Mount Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Sector.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Media Open 

#### ux_host_class_storage_media_open

**Icon** ![Host Class Storage Media Open icon](./media/user-guide/usbx-events/image179.png)

**Description**

This event represents a USBX Host Class Storage Media Open Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Media.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Media Read 

#### ux_host_class_storage_media_read

**Icon** ![Host Class Storage Media Read icon](./media/user-guide/usbx-events/image180.png)

**Description**

This event represents a USBX Host Class Storage Media Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Sector start.
- Info Field 3: Sector count.
- Info Field 4: Data pointer.

### Host Class Storage Media Write 

#### ux_host_class_storage_media_write

**Icon** ![Host Class Storage Media Write icon](./media/user-guide/usbx-events/image181.png)

**Description**

This event represents a USBX Host Class Storage Media Write Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Sector start.
- Info Field 3: Sector count.
- Info Field 4: Data pointer.

### Host Class Storage Request Sense 

#### ux_host_class_storage_request_sense

**Icon** ![Host Class Storage Request Sense icon](./media/user-guide/usbx-events/image182.png)

**Description**

This event represents a USBX Host Class Storage Request Sense Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Start Stop 

#### ux_host_class_storage_start_stop

**Icon** ![Host Class Storage Start Stop icon](./media/user-guide/usbx-events/image183.png)

**Description**

This event represents a USBX Host Class Storage Start Stop Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Start stop signal.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Unit Ready Test 

#### ux_host_class_storage_unit_ready_test

**Icon** ![Host Class Storage Unit Ready Test icon](./media/user-guide/usbx-events/image184.png)

**Description**

This event represents a USBX Host Class Storage Unit Ready Test Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Class Instance Create 

#### ux_host_class_instance_create

**Icon** ![Host Stack Class Instance Create icon](./media/user-guide/usbx-events/image185.png)

**Description**

This event represents a USBX Host Stack Class Instance Create Event.

**Information Fields**

- Info Field 1: Class.
- Info Field 2: Class Instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Class Instance Destroy 

#### ux_host_class_instance_create

**Icon** ![Host Stack Class Instance Destroy icon](./media/user-guide/usbx-events/image186.png)

**Description**

This event represents a USBX Host Stack Class Instance Destroy Event.

**Information Fields**

- Info Field 1: Class.
- Info Field 2: Class Instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Configuration Delete 

#### ux_host_class_configuration_delete

**Icon** ![Host Stack Configuration Delete icon](./media/user-guide/usbx-events/image187.png)

**Description**

This event represents a USBX Host Stack Configuration Delete Event.

**Information Fields**

- Info Field 1: Configuration.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Configuration Enumerate 

#### ux_host_stack_configuration_enumerate

**Icon** ![Host Stack Configuration Enumerate icon](./media/user-guide/usbx-events/image188.png)

**Description**

This event represents a USBX Host Stack Configuration Enumerate Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Stack Configuration Instance Create 

#### ux_host_stack_configuration_instance_create

**Icon** ![Stack Configuration Instance Create icon](./media/user-guide/usbx-events/image189.png)

**Description**

This event represents a USBX Host Stack Configuration Instance Create Event.

**Information Fields**

- Info Field 1: Configuration.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Configuration Instance Delete 

#### ux_host_stack_configuration_instance_delete

**Icon** ![Host Stack Configuration Instance Delete icon](./media/user-guide/usbx-events/image190.png)

**Description**

This event represents a USBX Host Stack Configuration Instance Delete Event.

**Information Fields**

- Info Field 1: Configuration.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Configuration Set 

#### ux_host_stack_configuration_set

**Icon** ![Host Stack Configuration Set icon](./media/user-guide/usbx-events/image191.png)

**Description**

This event represents a USBX Host Stack Configuration Set Event.

**Information Fields**

- Info Field 1: Configuration.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Address Set 

#### ux_host_stack_device_address_set

**Icon** ![Host Stack Device Address Set icon](./media/user-guide/usbx-events/image192.png)

**Description**

This event represents a USBX Host Stack Device Address Set Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Device Address.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Configuration Get 

#### ux_host_stack_device_configuration_get

**Icon** ![Host Stack Device Configuration Get icon](./media/user-guide/usbx-events/image193.png)

**Description**

This event represents a USBX Host Stack Device Configuration Get Event.

**Information Fields**

- Info Field 1: Device.
- nfo Field 2: Configuration.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Configuration Select 

#### ux_host_stack_device_configuration_select

**Icon** ![Host Stack Device Configuration Select icon](./media/user-guide/usbx-events/image194.png)

**Description**

This event represents a USBX Host Stack Device Configuration Select Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Configuration.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Descriptor Read 

#### ux_host_stack_device_descriptor_read

**Icon** ![Host Stack Device Descriptor Read icon](./media/user-guide/usbx-events/image195.png)

**Description**

This event represents a USBX Host Stack Device Descriptor Read Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Get 

#### ux_host_stack_device_get

**Icon** ![Host Stack Device Get icon](./media/user-guide/usbx-events/image196.png)

**Description**

This event represents a USBX Host Stack Device Get Event.

**Information Fields**

- Info Field 1: Device index.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Remove 

#### ux_host_stack_device_remove

**Icon** ![Host Stack Device Remove icon](./media/user-guide/usbx-events/image197.png)

**Description**

This event represents a USBX Host Stack Device Remove Event.

**Information Fields**

- Info Field 1: Hcd.
- Info Field 2: Parent.
- Info Field 3: Port Index.
- Info Field 4: Device.

### Host Stack Device Resource Free 

#### ux_host_stack_device_resource_free

**Icon** ![Host Stack Device Resource Free icon](./media/user-guide/usbx-events/image198.png)

**Description**

This event represents a USBX Host Stack Device Resource Free Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Endpoint Instance Create 

#### ux_host_stack_endpoint_instance_create

**Icon** ![Host Stack Endpoint Instance Create icon](./media/user-guide/usbx-events/image199.png)

**Description**

This event represents a USBX Host Stack Endpoint Instance Create Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Endpoint Instance Delete 

#### ux_host_stack_endpoint_instance_delete

**Icon** ![Host Stack Endpoint Instance Delete icon](./media/user-guide/usbx-events/image200.png)

**Description**

This event represents a USBX Host Stack Endpoint Instance Delete Event.

**Information Fields**

- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Endpoint Reset 

#### ux_host_stack_endpoint_reset

**Icon** ![Host Stack Endpoint Reset icon](./media/user-guide/usbx-events/image201.png)

**Description**

This event represents a USBX Host Stack Endpoint Reset Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Endpoint Transfer Abort 

#### ux_host_stack_endpoint_transfer_abort

**Icon** ![Host Stack Endpoint Transfer Abort icon](./media/user-guide/usbx-events/image202.png)

**Description**

This event represents a USBX Host Stack Endpoint Transfer Abort Event.

**Information Fields**

- Info Field 1: Endpoint.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Host Controller Register 

#### ux_host_stack_hcd_register

**Icon** ![Host Stack Host Controller Register icon](./media/user-guide/usbx-events/image203.png)

**Description**

This event represents a USBX Host Stack Host Controller Register.

**Information Fields**

- Info Field 1: Hcd Name.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Initialize 

#### ux_host_stack_initialize

**Icon** ![Host Stack Initialize icon](./media/user-guide/usbx-events/image204.png)

**Description**

This event represents a USBX Host Stack Initialize Event.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Endpoint Get 

#### Interface_ TCP retry entry

**Icon** ![Host Stack Interface Endpoint Get icon](./media/user-guide/usbx-events/image205.png)

**Description**

This event represents an internal NetX Duo TCP retry event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Endpoint index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Instance Create 

#### ux_host_stack_interface_instance_create

**Icon** ![Host Stack Interface Instance Create icon](./media/user-guide/usbx-events/image206.png)

**Description**

This event represents a USBX Host Stack Interface Instance Create Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Instance Delete 

#### ux_host_stack_interface_instance_delete

**Icon** ![Host Stack Interface Instance Delete icon](./media/user-guide/usbx-events/image207.png)

**Description**

This event represents a USBX Host Stack Interface Instance Delete Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Set 

#### ux_host_stack_interface_set

**Icon** ![Host Stack Interface Set icon](./media/user-guide/usbx-events/image208.png)

**Description**

This event represents a USBX Host Stack Interface Set Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Setting Select 

#### ux_host_stack_interface_setting_select

**Icon** ![Host Stack Interface Setting Select icon](./media/user-guide/usbx-events/image209.png)

**Description**

This event represents a USBX Host Stack Interface Setting Select Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack New Configuration Create 

#### ux_host_stack_new_configuration_create

**Icon** ![Host Stack New Configuration Create icon](./media/user-guide/usbx-events/image210.png)

**Description**
 
This event represents a USBX Host Stack New Configuration Create Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Configuration.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack New Device Create 

#### ux_host_stack_new_device_create

**Icon** ![Host Stack New Device Create icon](./media/user-guide/usbx-events/image211.png)

**Description**
 
 This event represents a USBX Host Stack New Device Create Event.

**Information Fields**

- Info Field 1: Hcd.
- Info Field 2: Device owner.
- Info Field 3: Port index.
- Info Field 4: Device.

### Host Stack New Endpoint Create 

#### ux_host_stack_new_endpoint_create

**Icon** ![Host Stack New Endpoint Create icon](./media/user-guide/usbx-events/image212.png)

**Description**
 
This event represents a USBX Host Stack New Endpoint Create Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Root Hub Change Process 

#### ux_host_stack_rh_change_process

**Icon** ![Host Stack Root Hub Change Process icon](./media/user-guide/usbx-events/image213.png)

**Description**
 
This event represents a USBX Host Stack Root Hub Change Process.

**Information Fields**

- Info Field 1: Port index.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Root Hub Device Extraction 

#### ux_host_stack_rh_device_extraction

**Icon** ![Host Stack Root Hub Device Extraction icon](./media/user-guide/usbx-events/image214.png)

**Description**

This event represents a USBX Host Stack Root Hub Device Extraction Event.

**Information Fields**

- Info Field 1: Hcd.
- Info Field 2: Port index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Root Hub Device Insertion 

#### ux_host_stack_rh_device_insertion

**Icon** ![Host Stack Root Hub Device Insertion icon](./media/user-guide/usbx-events/image215.png)

**Description**

This event represents a USBX Host Stack Root Hub Device Insertion.

**Information Fields**

- Info Field 1: Hcd.
- Info Field 2: Port index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Transfer Request 

#### ux_host_stack_transfer_request

**Icon** ![Host Stack Transfer Request icon](./media/user-guide/usbx-events/image216.png)

**Description**

This event represents a USBX Host Stack Transfer Request.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Endpoint.
- Info Field 3: Transfer request.
- Info Field 4: Not used.

### Host Stack Transfer Request Abort 

#### Internal I/O driver get status

**Icon** ![Host Stack Transfer Request Abort icon](./media/user-guide/usbx-events/image217.png)

**Description**

This event represents a USBX Host Stack Transfer Request Abort.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Endpoint.
- Info Field 3: Transfer request.
- Info Field 4: Not used.

### USBX Error 

#### ux_error

**Icon** ![U S B X Error icon](./media/user-guide/usbx-events/image218.png)

**Description**

This event represents a USBX Error Event.

**Information Fields**

- Info Field 1: USBX error.
- Info Field 2: Error Name.
- Info Field 3: Not used.
- Info Field 4: Not used.