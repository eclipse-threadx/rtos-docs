---
title: Chapter 9 - USBX trace events
description: This chapter contains a description of the USBX events displayed by TraceX. 
---


This chapter contains a description of the USBX events displayed by
TraceX. 

## List of Events and Icons

The following is a list of USBX events displayed by TraceX.

| Icon                             | Meaning                               |
| -------------------------------- | ------------------------------------- |
| {{< figure src="../media/user-guide/usbx-events/image1.png" title="Device Class C D C Activate icon" imgClass="img-responsive center-block" >}}    | **Device Class Cdc Activate** *(ux_device_class_cdc_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image2.png" title="Device Class C D C Deactivate icon" imgClass="img-responsive center-block" >}}    | **Device Class Cdc Deactivate** *(ux_device_class_cdc_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image3.png" title="Device Class C D C Read icon" imgClass="img-responsive center-block" >}}    | **Device Class Cdc Read** *(ux_device_class_cdc_read)* |
| {{< figure src="../media/user-guide/usbx-events/image4.png" title="Device Class C D C Write icon" imgClass="img-responsive center-block" >}}    | **Device Class Cdc Write** *(ux_device_class_cdc_write)* |
| {{< figure src="../media/user-guide/usbx-events/image5.png" title="Device Class Dpump Activate icon" imgClass="img-responsive center-block" >}}    | **Device Class Dpump Activate** *(ux_device_class_dpump_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image6.png" title="Device Class Dpump Deactivate icon" imgClass="img-responsive center-block" >}}    | **Device Class Dpump Deactivate** *(ux_device_class_dpump_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image7.png" title="Device Class Dpump Read icon" imgClass="img-responsive center-block" >}}    | **Device Class Dpump Read** *(ux_device_class_dpump_read)* |
| {{< figure src="../media/user-guide/usbx-events/image8.png" title="Device Class Dpump Write icon" imgClass="img-responsive center-block" >}}    | **Device Class Dpump Write** *(ux_device_class_dpump_write)* |
| {{< figure src="../media/user-guide/usbx-events/image9.png" title="Device Class Hid Activate icon" imgClass="img-responsive center-block" >}}    | **Device Class Hid Activate** *(ux_device_class_hid_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image10.png" title="Device Class Hid Deactivate icon" imgClass="img-responsive center-block" >}}    | **Device Class Hid Deactivate** *(ux_device_class_hid_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image11.png" title="Device Class Hid Descriptor Send icon" imgClass="img-responsive center-block" >}}    | **Device Class Hid Descriptor Send** *(ux_device_class_hid_descriptor_send)* |
| {{< figure src="../media/user-guide/usbx-events/image12.png" title="Device Class Hid Event Get icon" imgClass="img-responsive center-block" >}}    | **Device Class Hid Event Get** *(ux_device_class_hid_event_get)* |
| {{< figure src="../media/user-guide/usbx-events/image13.png" title="Device Class Hid Event Set icon" imgClass="img-responsive center-block" >}}    | **Device Class Hid Event Set** *(ux_device_class_hid_event_set)* |
| {{< figure src="../media/user-guide/usbx-events/image14.png" title="Device Class Hid Report Get icon" imgClass="img-responsive center-block" >}}    | **Device Class Hid Report Get** *(ux_device_class_hid_report_get)* |
| {{< figure src="../media/user-guide/usbx-events/image15.png" title="Device Class Hid Report Set icon" imgClass="img-responsive center-block" >}}    | **Device Class Hid Report Set** *(ux_device_class_hid_report_set)* |
| {{< figure src="../media/user-guide/usbx-events/image16.png" title="Device Class Pima Activate icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Activate** *(ux_device_class_pima_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image17.png" title="Device Class Pima Deactivate icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Deactivate** *(ux_device_class_pima_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image18.png" title="Device Class Pima Device Info Send icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Device Info Send** *(ux_device_class_pima_device_info_send)* |
| {{< figure src="../media/user-guide/usbx-events/image19.png" title="Device Class Pima Event Get icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Event Get** *(ux_device_class_pima_event_get)* |
| {{< figure src="../media/user-guide/usbx-events/image20.png" title="Device Class Pima Event Set icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Event Set** *(ux_device_class_pima_event_set)* |
| {{< figure src="../media/user-guide/usbx-events/image21.png" title="Device Class Pima Object Add icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Object Add** *(ux_device_class_pima_object_add)* |
| {{< figure src="../media/user-guide/usbx-events/image22.png" title="Device Class Pima Object Data Get icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Object Data Get** *(ux_device_class_pima_object_data_get)* |
| {{< figure src="../media/user-guide/usbx-events/image23.png" title="Device Class Pima Object Data Send icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Object Data Send** *(ux_device_class_pima_object_data_send)* |
| {{< figure src="../media/user-guide/usbx-events/image24.png" title="Device Class Pima Object Delete icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Object Delete** *(ux_device_class_pima_object_delete)* |
| {{< figure src="../media/user-guide/usbx-events/image25.png" title="Device Class Pima Object Handles Send icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Object Handles Send** *(ux_device_class_pima_object_handles_send)* |
| {{< figure src="../media/user-guide/usbx-events/image26.png" title="Device Class Pima Object Info Get icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Object Info Get** *(ux_device_class_pima_object_info_get)* |
| {{< figure src="../media/user-guide/usbx-events/image27.png" title="Device Class Pima Object Info Send icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Object Info Send** *(ux_device_class_pima_object_info_send)* |
| {{< figure src="../media/user-guide/usbx-events/image28.png" title="Device Class Pima Objects Number Send icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Objects Number Send** *(ux_device_class_pima_objects_number_send)* |
| {{< figure src="../media/user-guide/usbx-events/image29.png" title="Device Class Pima Partial Object Data Get icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Partial Object Data Get** *(ux_device_class_pima_partial_object_data_get)* |
| {{< figure src="../media/user-guide/usbx-events/image30.png" title="Device Class Pima Response Send icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Response Send** *(ux_device_class_pima_response_send)*|
| {{< figure src="../media/user-guide/usbx-events/image31.png" title="Device Class Pima Storage I D Send icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Storage Id Send** *(ux_device_class_pima_storage_id_send)* |
| {{< figure src="../media/user-guide/usbx-events/image32.png" title="Device Class Pima Storage Info Send icon" imgClass="img-responsive center-block" >}}    | **Device Class Pima Storage Info Send** *(ux_device_class_pima_storage_info_send)* |
| {{< figure src="../media/user-guide/usbx-events/image33.png" title="Device Class R N D I S Activate icon" imgClass="img-responsive center-block" >}}    | **Device Class Rndis Activate** *(ux_device_class_rndis_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image34.png" title="Device Class R N D I S Deactivate icon" imgClass="img-responsive center-block" >}}    | **Device Class Rndis Deactivate** *(ux_device_class_rndis_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image35.png" title="Device Class R N D I S Message Keep Aliveicon" imgClass="img-responsive center-block" >}}    | **Device Class Rndis Message Keep Alive** *(ux_device_class_rndis_msg_keep_alive)* |
| {{< figure src="../media/user-guide/usbx-events/image36.png" title="Device Class R N D I S Message Query icon" imgClass="img-responsive center-block" >}}    | **Device Class Rndis Message Query** *(ux_device_class_rndis_msg_query)* |
| {{< figure src="../media/user-guide/usbx-events/image37.png" title="Device Class R N D I S Message Reset icon" imgClass="img-responsive center-block" >}}    | **Device Class Rndis Message Reset** *(ux_device_class_rndis_msg_reset)* |
| {{< figure src="../media/user-guide/usbx-events/image38.png" title="Device Class R N D I S Message Set icon" imgClass="img-responsive center-block" >}}    | **Device Class Rndis Message Set** *(ux_device_class_rndis_msg_set)* |
| {{< figure src="../media/user-guide/usbx-events/image39.png" title="Device Class R N D I S Packet Receive icon" imgClass="img-responsive center-block" >}}    | **Device Class Rndis Packet Receive** *(ux_device_class_rndis_packet_receive)* |
| {{< figure src="../media/user-guide/usbx-events/image40.png" title="Device Class R N D I S Packet Transmit icon" imgClass="img-responsive center-block" >}}    | **Device Class Rndis Packet Transmit** *(ux_device_class_rndis_packet_transmit)* |
| {{< figure src="../media/user-guide/usbx-events/image41.png" title="Device Class Storage Activate icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Activate** *(ux_device_class_storage_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image42.png" title="Device Class Storage Deactivate icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Deactivate** *(ux_device_class_storage_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image43.png" title="Device Class Storage Format icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Format** *(ux_device_class_storage_format)* |
| {{< figure src="../media/user-guide/usbx-events/image44.png" title="Device Class Storage Inquiry icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Inquiry** *(ux_device_class_storage_inquiry)* |
| {{< figure src="../media/user-guide/usbx-events/image45.png" title="Device Class Storage Mode Select icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Mode Select** *(ux_device_class_storage_mode_select)* |
| {{< figure src="../media/user-guide/usbx-events/image46.png" title="Device Class Storage Mode Sense icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Mode Sense** *(ux_device_class_storage_mode_sense)* |
| {{< figure src="../media/user-guide/usbx-events/image47.png" title="Device Class Storage Prevent Allow Media Removal icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Prevent Allow Media Removal** *(ux_device_class_storage_prevent_allow_media_removal)* |
| {{< figure src="../media/user-guide/usbx-events/image48.png" title="Device Class Storage Read icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Read** *(ux_device_class_storage_read)* |
| {{< figure src="../media/user-guide/usbx-events/image49.png" title="Device Class Storage Read Capacity icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Read Capacity** *(ux_device_class_storage_read_capacity)* |
| {{< figure src="../media/user-guide/usbx-events/image50.png" title="Device Class Storage Read Format Capacity icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Read Format Capacity** *(ux_device_class_storage_read_format_capacity)* |
| {{< figure src="../media/user-guide/usbx-events/image51.png" title="Device Class Storage Read TOC icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Read TOC** *(ux_device_class_storage_read_toc)* |
| {{< figure src="../media/user-guide/usbx-events/image52.png" title="Device Class Storage Request Sense icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Request Sense** *(ux_device_class_storage_request_sense)* |
| {{< figure src="../media/user-guide/usbx-events/image53.png" title="Device Class Storage Start Stop icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Start Stop** *(ux_device_class_storage_start_stop)* |
| {{< figure src="../media/user-guide/usbx-events/image54.png" title="Device Class Storage Test Ready icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Test Ready** *(ux_device_class_storage_test_ready)* |
| {{< figure src="../media/user-guide/usbx-events/image55.png" title="Device Class Storage Verify icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Verify** *(ux_device_class_storage_verify)* |
| {{< figure src="../media/user-guide/usbx-events/image56.png" title="Device Class Storage Write icon" imgClass="img-responsive center-block" >}}    | **Device Class Storage Write** *(ux_device_class_storage_write)* |
| {{< figure src="../media/user-guide/usbx-events/image57.png" title="Device Stack Alternate Setting Get icon" imgClass="img-responsive center-block" >}}    | **Device Stack Alternate Setting Get** *(ux_device_stack_alternate_setting_get)* |
| {{< figure src="../media/user-guide/usbx-events/image58.png" title="Device Stack Alternate Setting Set icon" imgClass="img-responsive center-block" >}}    | **Device Stack Alternate Setting Set** *(ux_device_stack_alternate_setting_set)* |
| {{< figure src="../media/user-guide/usbx-events/image59.png" title="Device Stack Class Register icon" imgClass="img-responsive center-block" >}}    | **Device Stack Class Register** *(ux_device_stack_class_register)* |
| {{< figure src="../media/user-guide/usbx-events/image60.png" title="Device Stack Clear Feature icon" imgClass="img-responsive center-block" >}}    | **Device Stack Clear Feature** *(ux_device_stack_clear_feature)* |
| {{< figure src="../media/user-guide/usbx-events/image61.png" title="Device Stack Configuration Get icon" imgClass="img-responsive center-block" >}}    | **Device Stack Configuration Get** *(ux_device_stack_configuration_get)* |
| {{< figure src="../media/user-guide/usbx-events/image62.png" title="Device Stack Configuration Set icon" imgClass="img-responsive center-block" >}}    | **Device Stack Configuration Set** *(ux_device_stack_configuration_set)* |
| {{< figure src="../media/user-guide/usbx-events/image63.png" title="Device Stack Connect icon" imgClass="img-responsive center-block" >}}    | **Device Stack Connect** *(ux_device_stack_connect)* |
| {{< figure src="../media/user-guide/usbx-events/image64.png" title="Device Stack Descriptor Send icon" imgClass="img-responsive center-block" >}}    | **Device Stack Descriptor Send** *(ux_device_stack_descriptor_send)* |
| {{< figure src="../media/user-guide/usbx-events/image65.png" title="Device Stack Disconnect icon" imgClass="img-responsive center-block" >}}    | **Device Stack Disconnect** *(ux_device_stack_disconnect)* |
| {{< figure src="../media/user-guide/usbx-events/image66.png" title="Device Stack Endpoint Stall icon" imgClass="img-responsive center-block" >}}    | **Device Stack Endpoint Stall** *(ux_device_stack_endpoint_stall)* |
| {{< figure src="../media/user-guide/usbx-events/image67.png" title="Device Stack Get Status icon" imgClass="img-responsive center-block" >}}    | **Device Stack Get Status** *(ux_device_stack_get_status)* |
| {{< figure src="../media/user-guide/usbx-events/image68.png" title="Device Stack Host Wakeup icon" imgClass="img-responsive center-block" >}}    | **Device Stack Host Wakeup** *(ux_device_stack_host_wakeup)* |
| {{< figure src="../media/user-guide/usbx-events/image69.png" title="Device Stack Initialize icon" imgClass="img-responsive center-block" >}}    | **Device Stack Initialize** *(ux_device_stack_initialize)* |
| {{< figure src="../media/user-guide/usbx-events/image70.png" title="Device Stack Interface Delete icon" imgClass="img-responsive center-block" >}}    | **Device Stack Interface Delete** *(ux_device_stack_interface_delete)* |
| {{< figure src="../media/user-guide/usbx-events/image71.png" title="Device Stack Interface Get icon" imgClass="img-responsive center-block" >}}    | **Device Stack Interface Get** *(ux_device_stack_interface_get)* |
| {{< figure src="../media/user-guide/usbx-events/image72.png" title="Device Stack Interface Set icon" imgClass="img-responsive center-block" >}}    | **Device Stack Interface Set** *(ux_device_stack_interface_set)* |
| {{< figure src="../media/user-guide/usbx-events/image73.png" title="Device Stack Set Feature icon" imgClass="img-responsive center-block" >}}    | **Device Stack Set Feature** *(ux_device_stack_set_feature)* |
| {{< figure src="../media/user-guide/usbx-events/image74.png" title="Device Stack Transfer Abort icon" imgClass="img-responsive center-block" >}}    | **Device Stack Transfer Abort** *(ux_device_stack_transfer_abort)* |
| {{< figure src="../media/user-guide/usbx-events/image75.png" title="*Device Stack Transfer All Request Abort icon" imgClass="img-responsive center-block" >}}    | **Device Stack Transfer All Request Abort** *(ux_device_stack_transfer_all_request_abort)* |
| {{< figure src="../media/user-guide/usbx-events/image76.png" title="Device Stack Transfer Request icon" imgClass="img-responsive center-block" >}}    | **Device Stack Transfer Request** *(ux_device_stack_transfer_request)* |
| {{< figure src="../media/user-guide/usbx-events/image77.png" title="Host Class Asix Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Asix Activate** *(ux_host_class_asix_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image78.png" title="Host Class Asix Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Asix Deactivate** *(ux_host_class_asix_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image79.png" title="Host Class Asix Interrupt Notification icon" imgClass="img-responsive center-block" >}}    | **Host Class Asix Interrupt Notification** *(ux_host_class_asix_interrupt_notification)* |
| {{< figure src="../media/user-guide/usbx-events/image80.png" title="Host Class Asix Read icon" imgClass="img-responsive center-block" >}}    | **Host Class Asix Read** *(ux_host_class_asix_read)* |
| {{< figure src="../media/user-guide/usbx-events/image81.png" title="Host Class Asix Write icon" imgClass="img-responsive center-block" >}}    | **Host Class Asix Write** *(ux_host_class_asix_write)* |
| {{< figure src="../media/user-guide/usbx-events/image82.png" title="Host Class Audio Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Audio Activate** *(ux_host_class_audio_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image83.png" title="Host Class Audio Control Value Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Audio Control Value Get** *(ux_host_class_audio_control_value_get)* |
| {{< figure src="../media/user-guide/usbx-events/image84.png" title="Host Class Audio Control Value Set icon" imgClass="img-responsive center-block" >}}    | **Host Class Audio Control Value Set** *(ux_host_class_audio_control_value_set)* |
| {{< figure src="../media/user-guide/usbx-events/image85.png" title="Host Class Audio Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Audio Deactivate** *(ux_host_class_audio_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image86.png" title="Host Class Audio Read icon" imgClass="img-responsive center-block" >}}    | **Host Class Audio Read** *(ux_host_class_audio_read)* |
| {{< figure src="../media/user-guide/usbx-events/image87.png" title="Host Class Audio Streaming Sampling Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Audio Streaming Sampling Get** *(ux_host_class_audio_streaming_sampling_get)* |
| {{< figure src="../media/user-guide/usbx-events/image88.png" title="Host Class Audio Streaming Sampling Set icon" imgClass="img-responsive center-block" >}}    | **Host Class Audio Streaming Sampling Set** *(ux_host_class_audio_streaming_sampling_set)* |
| {{< figure src="../media/user-guide/usbx-events/image89.png" title="Host Class Audio Write icon" imgClass="img-responsive center-block" >}}    | **Host Class Audio Write** *(ux_host_class_audio_write)* |
| {{< figure src="../media/user-guide/usbx-events/image90.png" title="Host Class C D C A C M Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Activate** *(ux_host_class_cdc_acm_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image91.png" title="Host Class C D C A C M Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Deactivate** *(ux_host_class_cdc_acm_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image92.png" title="Host Class C D C A C M I O C T L In Pipe icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Ioctl Abort In Pipe** *(ux_host_class_cdc_acm_ioctl_abort_in_pipe)* |
| {{< figure src="../media/user-guide/usbx-events/image93.png" title="Host Class C D C A C M I O C T L Abort Out Pipe icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Ioctl Abort Out Pipe** *(ux_host_class_cdc_acm_ioctl_abort_out_pipe)* |
| {{< figure src="../media/user-guide/usbx-events/image94.png" title="Host Class C D C A C M I O C T L Get Device Status icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Ioctl Get Device Status** *(ux_host_class_cdc_acm_ioctl_get_device_status)* |
| {{< figure src="../media/user-guide/usbx-events/image95.png" title="Host Class C D C A C M I O C T L Get Line Coding icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Ioctl Get Line Coding** *(ux_host_class_cdc_acm_ioctl_get_line_coding)* |
| {{< figure src="../media/user-guide/usbx-events/image96.png" title="Host Class C D C A C M I O C T L Notification Callback icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Ioctl Notification Callback** *(ux_host_class_cdc_acm_ioctl_notification_callback)* |
| {{< figure src="../media/user-guide/usbx-events/image97.png" title="Host Class C D C A C M I O C T L Send Break icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Ioctl Send Break** *(ux_host_class_cdc_acm_ioctl_send_break)* |
| {{< figure src="../media/user-guide/usbx-events/image98.png" title="Host Class C D C A C M I O C T L Set Line Coding icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Ioctl Set Line Coding** *(ux_host_class_cdc_acm_ioctl_set_line_coding)* |
| {{< figure src="../media/user-guide/usbx-events/image99.png" title="Host Class C D C A C M I O C T L Set Line State icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Ioctl Set Line State** *(ux_host_class_cdc_acm_ioctl_set_line_state)* |
| {{< figure src="../media/user-guide/usbx-events/image100.png" title="Host Class C D C A C M Read icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Read** *(ux_host_class_cdc_acm_read)* |
| {{< figure src="../media/user-guide/usbx-events/image101.png" title="Host Class C D C A C M Reception Start icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Reception Start** *(ux_host_class_cdc_acm_reception_start)* |
| {{< figure src="../media/user-guide/usbx-events/image102.png" title="Host Class C D C A C M Reception Stop icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Reception Stop** *(ux_host_class_cdc_acm_reception_stop)* |
| {{< figure src="../media/user-guide/usbx-events/image103.png" title="Host Class C D C A C M Write icon" imgClass="img-responsive center-block" >}}    | **Host Class Cdc Acm Write** *(ux_host_class_cdc_acm_write)* |
| {{< figure src="../media/user-guide/usbx-events/image104.png" title="Host Class Dpump Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Dpump Activate** *(ux_host_class_dpump_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image105.png" title="Host Class Dpump Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Dpump Deactivate** *(ux_host_class_dpump_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image106.png" title="Host Class Dpump Read icon" imgClass="img-responsive center-block" >}}    | **Host Class Dpump Read** *(ux_host_class_dpump_read)* |
| {{< figure src="../media/user-guide/usbx-events/image107.png" title="Host Class Dpump Write icon" imgClass="img-responsive center-block" >}}    | **Host Class Dpump Write** *(ux_host_class_dpump_write)* |
| {{< figure src="../media/user-guide/usbx-events/image108.png" title="Host Class Hid Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Activate** *(ux_host_class_hid_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image109.png" title="Host Class Hid Client Register icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Client Register** *(ux_host_class_hid_client_register)* |
| {{< figure src="../media/user-guide/usbx-events/image110.png" title="Host Class Hid Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Deactivate** *(ux_host_class_hid_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image111.png" title="Host Class Hid Idle Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Idle Get** *(ux_host_class_hid_idle_get)* |
| {{< figure src="../media/user-guide/usbx-events/image112.png" title="Host Class Hid Idle Set icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Idle Set** *(ux_host_class_hid_idle_set)* |
| {{< figure src="../media/user-guide/usbx-events/image113.png" title="Host Class Hid Keyboard Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Keyboard Activate** *(ux_host_class_hid_keyboard_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image114.png" title="Host Class Hid Keyboard Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Keyboard Deactivate** *(ux_host_class_hid_keyboard_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image115.png" title="Host Class Hid Mouse Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Mouse Activate** *(ux_host_class_hid_mouse_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image116.png" title="Host Class Hid Mouse Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Mouse Deactivate** *(ux_host_class_hid_mouse_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image117.png" title="Host Class Hid Remote Control Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Remote Control Activate** *(ux_host_class_hid_remote_control_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image118.png" title="Host Class Hid Remote Control Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Remote Control Deactivate** *(ux_host_class_hid_remote_control_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image119.png" title="Host Class Hid Report Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Report Get** *(ux_host_class_hid_report_get)* |
| {{< figure src="../media/user-guide/usbx-events/image120.png" title="Host Class Hid Report Set icon" imgClass="img-responsive center-block" >}}    | **Host Class Hid Report Set** *(ux_host_class_hid_report_set)* |
| {{< figure src="../media/user-guide/usbx-events/image121.png" title="Host Class Hub Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hub Activate** *(ux_host_class_hub_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image122.png" title="Host Class Hub Change Detect icon" imgClass="img-responsive center-block" >}}    | **Host Class Hub Change Detect** *(ux_host_class_hub_change_detect)* |
| {{< figure src="../media/user-guide/usbx-events/image123.png" title="*Host Class Hub Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Hub Deactivate** *(ux_host_class_hub_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image124.png" title="Host Class Hub Port Change Connection Process icon" imgClass="img-responsive center-block" >}}    | **Host Class Hub Port Change Connection Process** *(ux_host_class_hub_port_change_connection_process)* |
| {{< figure src="../media/user-guide/usbx-events/image125.png" title="Host Class Hub Port Change Enable Process icon" imgClass="img-responsive center-block" >}}    | **Host Class Hub Port Change Enable Process** *(ux_host_class_hub_port_change_enable_process)* |
| {{< figure src="../media/user-guide/usbx-events/image126.png" title="Host Class Hub Port Change Over Current Process icon" imgClass="img-responsive center-block" >}}    | **Host Class Hub Port Change Over Current Process** *(ux_host_class_hub_port_change_over_current_process)* |
| {{< figure src="../media/user-guide/usbx-events/image127.png" title="Host Class Hub Port Change Reset Process icon" imgClass="img-responsive center-block" >}}    | **Host Class Hub Port Change Reset Process** *(ux_host_class_hub_port_change_reset_process)* |
| {{< figure src="../media/user-guide/usbx-events/image128.png" title="Host Class Hub Port Change Suspend Process icon" imgClass="img-responsive center-block" >}}    | **Host Class Hub Port Change Suspend Process** *(ux_host_class_hub_port_change_suspend_process)* |
| {{< figure src="../media/user-guide/usbx-events/image129.png" title="Host Class Pima Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Activate** *(ux_host_class_prima_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image130.png" title="Host Class Pima Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Deactivate** *(ux_host_class_pima_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image131.png" title="Host Class Pima Device Info Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Device Info Get** *(ux_host_class_pima_device_info_get)* |
| {{< figure src="../media/user-guide/usbx-events/image132.png" title="Host Class Pima Device Reset icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Device Reset** *(ux_host_class_pima_device_reset)* |
| {{< figure src="../media/user-guide/usbx-events/image133.png" title="Host Class Pima Notification icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Notification** *(ux_host_class_pima_notification)* |
| {{< figure src="../media/user-guide/usbx-events/image134.png" title="Host Class Pima Number Objects Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Number Objects Get** *(ux_host_class_pima_num_objects_get)* |
| {{< figure src="../media/user-guide/usbx-events/image135.png" title="Host Class Pima Object Close icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Object Close** *(ux_host_class_pima_object_close)* |
| {{< figure src="../media/user-guide/usbx-events/image136.png" title="Host Class Pima Object Copy icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Object Copy** *(ux_host_class_pima_object_copy)* |
| {{< figure src="../media/user-guide/usbx-events/image137.png" title="Host Class Pima Object Delete icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Object Delete** *(ux_host_class_pima_object_delete)* |
| {{< figure src="../media/user-guide/usbx-events/image138.png" title="Host Class Pima Object Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Object Get** *(ux_host_class_pima_object_get)* |
| {{< figure src="../media/user-guide/usbx-events/image139.png" title="Host Class Pima Object Info Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Object Info Get** *(ux_host_class_pima_object_info_get)* |
| {{< figure src="../media/user-guide/usbx-events/image140.png" title="Host Class Pima Object Info Send icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Object Info Send** *(ux_host_class_pima_object_info_send)* |
| {{< figure src="../media/user-guide/usbx-events/image141.png" title="Host Class Pima Object Move icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Object Move** *(ux_host_class_pima_object_move)* |
| {{< figure src="../media/user-guide/usbx-events/image142.png" title="Host Class Pima Object Send icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Object Send** *(ux_host_class_pima_object_send)* |
| {{< figure src="../media/user-guide/usbx-events/image143.png" title="Host Class Pima Object Transfer Abort icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Object Transfer Abort** *(ux_host_class_object_transfer_abort)* |
| {{< figure src="../media/user-guide/usbx-events/image144.png" title="Host Class Pima Read icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Read** *(ux_host_class_pima_read)* |
| {{< figure src="../media/user-guide/usbx-events/image145.png" title="Host Class Pima Request Cancel icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Request Cancel** *(ux_host_class_pima_request_cancel)* |
| {{< figure src="../media/user-guide/usbx-events/image146.png" title="Host Class Pima Session Close icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Session Close** *(ux_host_class_pima_session_close)* |
| {{< figure src="../media/user-guide/usbx-events/image147.png" title="Host Class Pima Session Open icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Session Open** *(ux_host_class_pima_session_open)* |
| {{< figure src="../media/user-guide/usbx-events/image148.png" title="Host Class Pima Storage Ids Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Storage Ids Get** *(ux_host_class_pima_storage_ids_get)* |
| {{< figure src="../media/user-guide/usbx-events/image149.png" title="Host Class Pima Storage Info Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Storage Info Get** *(ux_host_class_pima_storage_info_get)* |
| {{< figure src="../media/user-guide/usbx-events/image150.png" title="Host Class Pima Thumb Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Thumb Get** *(ux_host_class_pima_thumb_get)* |
| {{< figure src="../media/user-guide/usbx-events/image151.png" title="Host Class Pima Write icon" imgClass="img-responsive center-block" >}}    | **Host Class Pima Write** *(ux_host_class_pima_write)* |
| {{< figure src="../media/user-guide/usbx-events/image152.png" title="Host Class Printer Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Printer Activate** *(ux_host_class_printer_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image153.png" title="Host Class Printer Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Printer Deactivate** *(ux_host_class_printer_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image154.png" title="Host Class Printer Name Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Printer Name Get** *(ux_host_class_printer_name_get)* |
| {{< figure src="../media/user-guide/usbx-events/image155.png" title="Host Class Printer Read icon" imgClass="img-responsive center-block" >}}    |  **Host Class Printer Read** *(ux_host_class_printer_read)* |
| {{< figure src="../media/user-guide/usbx-events/image156.png" title="Host Class Printer Soft Reset icon" imgClass="img-responsive center-block" >}}    | **Host Class Printer Soft Reset** *(ux_host_class_printer_soft_reset)* |
| {{< figure src="../media/user-guide/usbx-events/image157.png" title="Host Class Printer Status Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Printer Status Get** *(ux_host_class_printer_status_get)* |
| {{< figure src="../media/user-guide/usbx-events/image158.png" title="Host Class Printer Write icon" imgClass="img-responsive center-block" >}}    | **Host Class Printer Write** *(ux_host_class_printer_write)* |
| {{< figure src="../media/user-guide/usbx-events/image159.png" title="Host Class Prolific Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Activate** *(ux_host_class_prolific_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image160.png" title="Host Class Prolific Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Deactivate** *(ux_host_class_prolific_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image161.png" title="Host Class Prolific I O C T L Abort In Pipe icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Ioctl Abort In Pipe** *(ux_host_class_prolific_ioctl_abort_in_pipe)* |
| {{< figure src="../media/user-guide/usbx-events/image162.png" title="Host Class Prolific I O C T L Abort Out Pipe icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Ioctl Abort Out Pipe** *(ux_host_class_prolific_ioctl_abort_out_pipe)* |
| {{< figure src="../media/user-guide/usbx-events/image163.png" title="Host Class Prolific I O C T L Get Device Status icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Ioctl Get Device Status** *(ux_host_class_prolific_ioctl_get_device_status)* |
| {{< figure src="../media/user-guide/usbx-events/image164.png" title="Host Class Prolific I O C T L Get Line Coding icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Ioctl Get Line Coding** *(ux_host_class_prolific_ioctl_get_line_coding)* |
| {{< figure src="../media/user-guide/usbx-events/image165.png" title="Host Class Prolific I O C T L Purge icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Ioctl Purge** *(ux_host_class_prolific_ioctl_purge)* |
| {{< figure src="../media/user-guide/usbx-events/image166.png" title="Host Class Prolific I O C T L Report Device Status Change icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Ioctl Report Device Status Change** *(ux_host_class_prolific_ioctl_report_device_status_change)* |
| {{< figure src="../media/user-guide/usbx-events/image167.png" title="Host Class Prolific I O C T L Send Break icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Ioctl Send Break** *(ux_host_class_prolific_ioctl_send_break)* |
| {{< figure src="../media/user-guide/usbx-events/image168.png" title="Host Class Prolific I O C T L Set Line Coding icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Ioctl Set Line Coding** *(ux_host_class_prolific_ioctl_set_line_coding)* |
| {{< figure src="../media/user-guide/usbx-events/image169.png" title="Host Class Prolific I O C T L Set Line State icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Ioctl Set Line State** *(ux_host_class_prolific_ioctl_set_line_state)* |
| {{< figure src="../media/user-guide/usbx-events/image170.png" title="Host Class Prolific Read icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Read** *(ux_host_class_prolific_read)* |
| {{< figure src="../media/user-guide/usbx-events/image171.png" title="Host Class Prolific Reception Start icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Reception Start** *(ux_host_class_prolific_reception_start)* |
| {{< figure src="../media/user-guide/usbx-events/image172.png" title="Host Class Prolific Reception Stop icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Reception Stop** *(ux_host_class_prolific_reception_stop)* |
| {{< figure src="../media/user-guide/usbx-events/image173.png" title="Host Class Prolific Write icon" imgClass="img-responsive center-block" >}}    | **Host Class Prolific Write** *(ux_host_class_prolific_write)* |
| {{< figure src="../media/user-guide/usbx-events/image174.png" title="Host Class Storage Activate icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Activate** *(ux_host_class_storage_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image175.png" title="Host Class Storage Deactivate icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Deactivate** (*ux_host_class_storage_deactivate)* |
| {{< figure src="../media/user-guide/usbx-events/image176.png" title="Host Class Storage Media Capacity Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Media Capacity Get** *(ux_host_class_storage_media_capacity_get)* |
| {{< figure src="../media/user-guide/usbx-events/image177.png" title="Host Class Storage Media Format Capacity Get icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Media Format Capacity Get** *(ux_host_class_storage_media_format_capacity_get)* |
| {{< figure src="../media/user-guide/usbx-events/image178.png" title="Host Class Storage Media Mount icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Media Mount** (ux_host_class_storage_media_mount)* |
| {{< figure src="../media/user-guide/usbx-events/image179.png" title="Host Class Storage Media Open icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Media Open** *(ux_host_class_storage_media_open)* |
| {{< figure src="../media/user-guide/usbx-events/image180.png" title="Host Class Storage Media Read icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Media Read** *(ux_host_class_storage_media_read)* |
| {{< figure src="../media/user-guide/usbx-events/image181.png" title="Host Class Storage Media Write icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Media Write** *(ux_host_class_storage_media_write)* |
| {{< figure src="../media/user-guide/usbx-events/image182.png" title="Host Class Storage Request Sense icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Request Sense** *(ux_host_class_storage_request_sense)* |
| {{< figure src="../media/user-guide/usbx-events/image183.png" title="Host Class Storage Start Stop icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Start Stop** *(ux_host_class_storage_start_stop)* |
| {{< figure src="../media/user-guide/usbx-events/image184.png" title="Host Class Storage Unit Ready Test icon" imgClass="img-responsive center-block" >}}    | **Host Class Storage Unit Ready Test** *(ux_host_class_storage_activate)* |
| {{< figure src="../media/user-guide/usbx-events/image185.png" title="Host Stack Class Instance Create icon" imgClass="img-responsive center-block" >}}    | **Host Stack Class Instance Create** *(ux_host_stack_class_instance_create)* |
| {{< figure src="../media/user-guide/usbx-events/image186.png" title="Host Stack Class Instance Destroy icon" imgClass="img-responsive center-block" >}}    | **Host Stack Class Instance Destroy** *(ux_host_stack_class_instance_destroy)* |
| {{< figure src="../media/user-guide/usbx-events/image187.png" title="Host Stack Configuration Delete icon" imgClass="img-responsive center-block" >}}    | **Host Stack Configuration Delete** *(ux_host_stack_configuration_delete)* |
| {{< figure src="../media/user-guide/usbx-events/image188.png" title="Host Stack Configuration Enumerate icon" imgClass="img-responsive center-block" >}}    | **Host Stack Configuration Enumerate** *(ux_host_stack_configuration_enumerate)* |
| {{< figure src="../media/user-guide/usbx-events/image189.png" title="Host Stack Configuration Instance Create icon" imgClass="img-responsive center-block" >}}    | **Host Stack Configuration Instance Create** *(ux_host_stack_configuration_instance_create)* |
| {{< figure src="../media/user-guide/usbx-events/image190.png" title="Host Stack Configuration Instance Delete icon" imgClass="img-responsive center-block" >}}    | **Host Stack Configuration Instance Delete** *(ux_host_stack_configuration_instance_delete)* |
| {{< figure src="../media/user-guide/usbx-events/image191.png" title="Host Stack Configuration Set icon" imgClass="img-responsive center-block" >}}    | **Host Stack Configuration Set** *(ux_host_stack_configuration_set)* |
| {{< figure src="../media/user-guide/usbx-events/image192.png" title="Host Stack Device Address Set icon" imgClass="img-responsive center-block" >}}    | **Host Stack Device Address Set** *(ux_host_stack_device_set)* |
| {{< figure src="../media/user-guide/usbx-events/image193.png" title="Host Stack Device Configuration Get icon" imgClass="img-responsive center-block" >}}    | **Host Stack Device Configuration Get** *(ux_host_stack_device_configuration_get)* |
| {{< figure src="../media/user-guide/usbx-events/image194.png" title="Host Stack Device Configuration Select icon" imgClass="img-responsive center-block" >}}    | **Host Stack Device Configuration Select** *(ux_host_stack_device_configuration_select)* |
| {{< figure src="../media/user-guide/usbx-events/image195.png" title="Host Stack Device Descriptor Read icon" imgClass="img-responsive center-block" >}}    | **Host Stack Device Descriptor Read** *(ux_host_stack_device_descriptor_read)* |
| {{< figure src="../media/user-guide/usbx-events/image196.png" title="Host Stack Device Get icon" imgClass="img-responsive center-block" >}}    | **Host Stack Device Get** (ux_host_stack_device_get) |
| {{< figure src="../media/user-guide/usbx-events/image197.png" title="Host Stack Device Remove icon" imgClass="img-responsive center-block" >}}    | **Host Stack Device Remove** (ux_host_stack_device_get) |
| {{< figure src="../media/user-guide/usbx-events/image198.png" title="Host Stack Device Resource Free icon" imgClass="img-responsive center-block" >}}    | **Host Stack Device Resource Free** (ux_host_stack_device_resource_free) |
| {{< figure src="../media/user-guide/usbx-events/image199.png" title="Host Stack Endpoint Instance Create icon" imgClass="img-responsive center-block" >}}    | **Host Stack Endpoint Instance Create** (ux_host_stack_endpoint_instance_create) |
| {{< figure src="../media/user-guide/usbx-events/image200.png" title="Host Stack Endpoint Instance Delete icon" imgClass="img-responsive center-block" >}}    | **Host Stack Endpoint Instance Delete** (ux_host_stack_endpoint_instance_delete) |
| {{< figure src="../media/user-guide/usbx-events/image201.png" title="Host Stack Endpoint Reset icon" imgClass="img-responsive center-block" >}}    | **Host Stack Endpoint Reset** (ux_host_stack_endpoint_reset) |
| {{< figure src="../media/user-guide/usbx-events/image202.png" title="Host Stack Endpoint Transfer Abort icon" imgClass="img-responsive center-block" >}}    | **Host Stack Endpoint Transfer Abort** (ux_host_stack_endpoint_transfer_abort) |
| {{< figure src="../media/user-guide/usbx-events/image203.png" title="Host Stack Host Controller Register icon" imgClass="img-responsive center-block" >}}    | **Host Stack Host Controller Register** *(ux_host_stack_hcd_register)* |
| {{< figure src="../media/user-guide/usbx-events/image204.png" title="Host Stack Initialize icon" imgClass="img-responsive center-block" >}}    | **Host Stack Initialize** *(ux_host_stack_initialize)* |
| {{< figure src="../media/user-guide/usbx-events/image205.png" title="Host Stack Interface Endpoint Get icon" imgClass="img-responsive center-block" >}}    | **Host Stack Interface Endpoint Get** *(ux_host_stack_interface_endpoint_get)* |
| {{< figure src="../media/user-guide/usbx-events/image206.png" title="Host Stack Interface Instance Create icon" imgClass="img-responsive center-block" >}}    | **Host Stack Interface Instance Create** *(ux_host_stack_interface_instance_create)* |
| {{< figure src="../media/user-guide/usbx-events/image207.png" title="Host Stack Interface Instance Delete icon" imgClass="img-responsive center-block" >}}    | **Host Stack Interface Instance Delete** *(ux_host_stack_interface_instance_delete)* |
| {{< figure src="../media/user-guide/usbx-events/image208.png" title="Host Stack Interface Set icon" imgClass="img-responsive center-block" >}}    | **Host Stack Interface Set** *(ux_host_stack_interface_set)* |
| {{< figure src="../media/user-guide/usbx-events/image209.png" title="Host Stack Interface Setting Select icon" imgClass="img-responsive center-block" >}}    | **Host Stack Interface Setting Select** *(ux_host_stack_interface_setting_select)* |
| {{< figure src="../media/user-guide/usbx-events/image210.png" title="Host Stack New Configuration Create icon" imgClass="img-responsive center-block" >}}    | **Host Stack New Configuration Create** *(ux_host_stack_new_configuration_create)* |
| {{< figure src="../media/user-guide/usbx-events/image211.png" title="Host Stack New Device Create icon" imgClass="img-responsive center-block" >}}    | **Host Stack New Device Create** *(ux_host_stack_new_device_create)* |
| {{< figure src="../media/user-guide/usbx-events/image212.png" title="Host Stack New Endpoint Create icon" imgClass="img-responsive center-block" >}}    | **Host Stack New Endpoint Create** *(ux_host_stack_new_endpoint_create)* |
| {{< figure src="../media/user-guide/usbx-events/image213.png" title="Host Stack Root Hub Change Process icon" imgClass="img-responsive center-block" >}}    | **Host Stack Root Hub Change Process** *(ux_host_stack_rh_change_process)* |
| {{< figure src="../media/user-guide/usbx-events/image214.png" title="Host Stack Root Hub Device Extraction icon" imgClass="img-responsive center-block" >}}    | **Host Stack Root Hub Device Extraction** *(ux_host_stack_rh_device_extraction)* |
| {{< figure src="../media/user-guide/usbx-events/image215.png" title="Host Stack Root Hub Device Insertion icon" imgClass="img-responsive center-block" >}}    | **Host Stack Root Hub Device Insertion** *(ux_host_stack_rh_device_insertion)* |
| {{< figure src="../media/user-guide/usbx-events/image216.png" title="Host Stack Transfer Request icon" imgClass="img-responsive center-block" >}}    | **Host Stack Transfer Request** *(ux_host_stack_transfer_request)* |
| {{< figure src="../media/user-guide/usbx-events/image217.png" title="Host Stack Transfer Request Abort icon" imgClass="img-responsive center-block" >}}    | **Host Stack Transfer Request Abort** *(ux_host_stack_transfer_request_abort)* |
| {{< figure src="../media/user-guide/usbx-events/image218.png" title="U S B X Error icon" imgClass="img-responsive center-block" >}}    | **USBX Error** *(ux_error)* |

## Event Descriptions

The following pages describe the USBX Trace Events.

### Device Class Cdc Activate 

#### ux_device_class_cdc_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image1.png" title="Device Class C D C Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Cdc Activate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Cdc Deactivate 

#### ux_device_class_cdc_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image2.png" title="Device Class C D C Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Cdc Deactivate.

Information Fields 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Cdc Read 

#### ux_device_class_cdc_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image3.png" title="Device Class C D C Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Cdc Read Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Device Class Cdc Write 

#### ux_device_class_cdc_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image4.png" title="Device Class C D C Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Cdc Write Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Device Class Dpump Activate 

#### ux_device_class_dpump_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image5.png" title="Device Class Dpump Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Dpump Activate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Dpump Deactivate 

#### ux_device_class_dpump_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image6.png" title="Device Class Dpump Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Dpump Deactivate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Dpump Read 

#### ux_device_class_dpump_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image7.png" title="Device Class Dpump Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Dpump Read Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Buffer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Device Class Dpump Write 

#### ux_device_class_dpump_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image8.png" title="Device Class Dpump Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Dpump Write Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Device Class Hid Activate 

#### ux_device_class_hid_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image9.png" title="Device Class Hid Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Hid Activate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Hid Deactivate 

#### ux_device_class_hid_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image10.png" title="Device Class Hid Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Hid Deactivate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Hid Descriptor Send 

#### ux_device_class_hid_descriptor_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image11.png" title="Device Class Hid Descriptor Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Hid Descriptor Send Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Descriptor type.
- Info Field 3: Request index.
- Info Field 4: Not used.

### Device Class Hid Event Get 

#### ux_device_class_hid_event_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image12.png" title="Device Class Hid Event Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Hid Event Get Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Hid Event Set 

#### ux_device_class_hid_event_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image13.png" title="Device Class Hid Event Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Hid Event Set.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Hid Report Get 

#### ux_device_class_hid_report_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image14.png" title="Device Class Hid Report Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Hid Report Get Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Descriptor type.
- Info Field 3: Request index.
- Info Field 4: Not used.

### Device Class Hid Report Set 

#### ux_device_class_hid_report_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image15.png" title="Device Class Hid Report Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Hid Report Set Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Descriptor type.
- Info Field 3: Request index.
- Info Field 4: Not used.

### Device Class Pima Activate

#### ux_device_class_pima_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image16.png" title="Device Class Pima Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Activate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Deactivate 

#### ux_device_class_pima_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image17.png" title="Device Class Pima Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Deactivate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Device Info Send 

#### ux_device_class_pima_device_info_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image18.png" title="Device Class Pima Device Info Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Device Info Send Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.

### Device Class Pima Event Get 

#### ux_device_class_pima_event_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image19.png" title="Device Class Pima Event Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Event Get Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Pima event.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Event Set 

#### ux_device_class_pima_event_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image20.png" title="Device Class Pima Event Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Event Set Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Pima event.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Add 

#### ux_device_class_pima_object_add

**Icon** {{< figure src="../media/user-guide/usbx-events/image21.png" title="Device Class Pima Object Add icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Object Add Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Data Get 

#### ux_device_class_pima_object_data_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image22.png" title="Device Class Pima Object Data Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Object Data Get Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Data Send 

#### ux_device_class_pima_object_data_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image23.png" title="Device Class Pima Object Data Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Object Data Send Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Delete 

#### ux_device_class_pima_object_delete

**Icon** {{< figure src="../media/user-guide/usbx-events/image24.png" title="Device Class Pima Object Delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Object Delete Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Handles Send 

#### ux_device_class_pima_object_handles_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image25.png" title="Device Class Pima Object Handles Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Object Handles Send Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Storage ID.
- Info Field 3: Object format code.
- Info Field 4: Object association.

### Device Class Pima Object Info Get 

#### ux_device_class_pima_object_info_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image26.png" title="Device Class Pima Object Info Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Object Info Get Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Object Info Send 

#### ux_device_class_pima_object_info_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image27.png" title="Device Class Pima Object Info Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Object Info Send Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Objects Number Send 

#### ux_device_class_pima_object_number_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image28.png" title="Device Class Pima Objects Number Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Object Number Send event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Storage ID.
- Info Field 3: Object format code.
- Info Field 4: Object associate.

### Device Class Pima Partial Object Data Get

#### ux_device_class_pima_partial_object_data_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image29.png" title="Device Class Pima Partial Object Data Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Partial Object Data Get Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Object handle.
- Info Field 3: Offset requested.
- Info Field 4: Length requested.

### Device Class Pima Response Send 

#### ux_device_class_pima_response_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image30.png" title="Device Class Pima Response Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Response Send Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Response code.
- Info Field 3: Number parameter.
- Info Field 4: Pima parameter 1.

### Device Class Pima Storage Id Send 

#### ux_device_class_pima_storage_id_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image31.png" title="Device Class Pima Storage Id Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Storage Id Send Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Pima Storage Info Send 

#### ux_device_class_pima_storage_info_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image32.png" title="Device Class Pima Storage Info Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Pima Storage Info Send Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Activate 

#### ux_device_class_rndis_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image33.png" title="Device Class Rndis Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Rndis Activate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Deactivate 

#### ux_device_class_rndis_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image34.png" title="Device Class Rndis Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Rndis Deactivate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Message Keep Alive 

#### ux_device_class_rndis_msg_keep_alive

**Icon** {{< figure src="../media/user-guide/usbx-events/image35.png" title="Device Class Rndis Message Keep Alive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Rndis Message Keep Alive Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Message Query 

#### ux_device_class_rndis_msg_keep_query

**Icon** {{< figure src="../media/user-guide/usbx-events/image36.png" title="Device Class Rndis Message Query icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Rndis Message Query Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Rndis OID.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Message Reset 

#### ux_device_class_rndis_msg_reset

**Icon** {{< figure src="../media/user-guide/usbx-events/image37.png" title="Device Class Rndis Message Reset icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Rndis Message Reset Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.

### Device Class Rndis Message Set 

#### ux_device_class_rndis_msg_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image38.png" title="Device Class Rndis Message Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Rndis Message Set Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Rndis OID.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Packet Receive 

#### ux_device_class_rndis_packet_receive

**Icon** {{< figure src="../media/user-guide/usbx-events/image39.png" title="Device Class Rndis Packet Receive icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Rndis Packet Receive Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Rndis Packet Transmit 

#### ux_device_class_rndis_packet_transmit

**Icon** {{< figure src="../media/user-guide/usbx-events/image40.png" title="Device Class Rndis Packet Transmit icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Rndis Packet Transmit Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Activate 

#### ux_device_class_storage_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image41.png" title="Device Class Storage Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Activate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Deactivate 

#### ux_device_class_storage_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image42.png" title="Device Class Storage Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Deactivate Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Format 

#### ux_device_class_storage_format

**Icon** {{< figure src="../media/user-guide/usbx-events/image43.png" title="Device Class Storage Format icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Format Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Inquiry 

#### ux_device_class_storage_inquiry

**Icon** {{< figure src="../media/user-guide/usbx-events/image44.png" title="Device Class Storage Inquiry icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Inquiry Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Mode Select

#### ux_device_class_storage_mode_select

**Icon** {{< figure src="../media/user-guide/usbx-events/image45.png" title="Device Class Storage Mode Select icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Mode Select Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Mode Sense 

#### ux_device_class_storage_mode_sense

**Icon** {{< figure src="../media/user-guide/usbx-events/image46.png" title="Device Class Storage Mode Sense icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Mode Sense Event.

Information Fields 

- nfo Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Prevent Allow Media Removal 

#### ux_device_class_storage_prevent_allow_media_removal

**Icon** {{< figure src="../media/user-guide/usbx-events/image47.png" title="Device Class Storage Prevent Allow Media Removal icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Prevent Allow Media Removal Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Read 

#### ux_device_class_storage_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image48.png" title="Device Class Storage Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Read Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Sector.
- Info Field 4: Number sectors.

### Device Class Storage Read Capacity 

#### ux_device_class_storage_read_capacity

**Icon** {{< figure src="../media/user-guide/usbx-events/image49.png" title="Device Class Storage Read Capacity icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Read Capacity Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Read Format Capacity 

#### ux_device_class_storage_read_format_capacity

**Icon** {{< figure src="../media/user-guide/usbx-events/image50.png" title="Device Class Storage Read Format Capacity icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Read Format Capacity Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Read TOC 

#### ux_device_class_storage_read_toc

**Icon** {{< figure src="../media/user-guide/usbx-events/image51.png" title="Device Class Storage Read TOC icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Read TOC Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Request Sense 

#### ux_device_class_storage_request_sense

**Icon** {{< figure src="../media/user-guide/usbx-events/image52.png" title="Device Class Storage Request Sense icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Request Sense Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Sense key.
- Info Field 4: Code.

### Device Class Storage Start Stop 

#### ux_device_class_storage_start_stop

**Icon** {{< figure src="../media/user-guide/usbx-events/image53.png" title="Device Class Storage Start Stop icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Start Stop Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Test Ready 

#### ux_device_class_storage_test_ready

**Icon** {{< figure src="../media/user-guide/usbx-events/image54.png" title="Device Class Storage Test Ready icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Test Ready Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Verify 

#### ux_device_class_storage_verify

**Icon** {{< figure src="../media/user-guide/usbx-events/image55.png" title="Device Class Storage Verify icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Verify Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Class Storage Write 

#### ux_device_class_storage_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image56.png" title="Device Class Storage Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Class Storage Write Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Lun.
- Info Field 3: Sector.
- Info Field 4: Number sectors.

### Device Stack Alternate Setting Get 

#### ux_device_class_alternate_setting_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image57.png" title="Device Stack Alternate Setting Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Alternate Setting Get Event.

**Information Fields**

- Info Field 1: Interface value.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Alternate Setting Set 

#### ux_device_class_alternate_setting_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image58.png" title="Device Stack Alternate Setting Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Alternate Setting Set Event.

**Information Fields**
- Info Field 1: Interface value.
- Info Field 2: Alternate setting value.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Class Register 

#### ux_device_stack_class_register

**Icon** {{< figure src="../media/user-guide/usbx-events/image59.png" title="Device Stack Class Register icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Class Register Event.

**Information Fields**

- Info Field 1: Class name.
- Info Field 2: Interface number.
- Info Field 3: Parameter.
- Info Field 4: Not used.

### Device Stack Clear Feature 

#### ux_device_stack_clear_feature

**Icon** {{< figure src="../media/user-guide/usbx-events/image60.png" title="Device Stack Clear Feature icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Clear Feature Event.

**Information Fields**

- Info Field 1: Request type.
- Info Field 2: Request value. Info Field 3: Request index.
- Info Field 4: Not used.

### Device Stack Configuration Get 

#### ux_device_stack_configuration_get t

**Icon** {{< figure src="../media/user-guide/usbx-events/image61.png" title="Device Stack Configuration Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Configuration Get Event.

**Information Fields** 

- Info Field 1: Configuration value.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Configuration Set 

#### ux_device_stack_configuration_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image62.png" title="Device Stack Configuration Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Configuration Set Event.

**Information Fields** 

- Info Field 1: Configuration value.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Connect 

#### ux_device_stack_connect

**Icon** {{< figure src="../media/user-guide/usbx-events/image63.png" title="Device Stack Connect icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Descriptor Send Event.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Descriptor Send 

#### ux_device_stack_descriptor_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image64.png" title="Device Stack Descriptor Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Descriptor Send Event.

**Information Fields**

- Info Field 1: Descriptor type.
- Info Field 2: Request index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Disconnect 

#### ux_device_stack_disconnect

**Icon** {{< figure src="../media/user-guide/usbx-events/image65.png" title="Device Stack Disconnect icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Disconnect Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Endpoint Stall 

#### ux_device_stack_endpoint_stall

**Icon** {{< figure src="../media/user-guide/usbx-events/image66.png" title="Device Stack Endpoint Stall icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Endpoint Stall Event.

**Information Fields** 

- Info Field 1: Endpoint.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Get Status 

#### ux_device_stack_get_status

**Icon** {{< figure src="../media/user-guide/usbx-events/image67.png" title="Device Stack Get Status icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Get Status Event.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Host Wakeup 

#### ux_device_stack_host_wakeup

**Icon** {{< figure src="../media/user-guide/usbx-events/image68.png" title="Device Stack Host Wakeup icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Host Wakeup Event.

**Information Fields** 

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Initialize 

#### ux_device_stack_initialize

**Icon** {{< figure src="../media/user-guide/usbx-events/image69.png" title="Device Stack Initialize icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Initialize Event.

**Information Fields** 

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Interface Delete 

#### ux_device_stack_interface_delete

**Icon** {{< figure src="../media/user-guide/usbx-events/image70.png" title="Device Stack Interface Delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Interface Delete Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Interface Get 

#### ux_device_stack_interface_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image71.png" title="Device Stack Interface Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Interface Get Event.

**Information Fields** 

- Info Field 1: Interface value.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Interface Set 

#### ux_device_stack_interface_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image72.png" title="Device Stack Interface Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Interface Set Event.

**Information Fields** 

- Info Field 1: Request value. Info Field 2: Request index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Set Feature 

#### ux_device_stack_set_feature

**Icon** {{< figure src="../media/user-guide/usbx-events/image73.png" title="Device Stack Set Feature icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Set Feature Event.

**Information Fields** 

- Info Field 1: Request value. Info Field 2: Request index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Transfer Abort 

#### ux_device_stack_transfer_abort

**Icon** {{< figure src="../media/user-guide/usbx-events/image74.png" title="Device Stack Transfer Abort icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Transfer Abort Event.

**Information Fields** 

- Info Field 1: Transfer request.
- Info Field 2: Completion code.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Transfer All Request Abort 

#### ux_device_stack_transfer_all_request_abort

**Icon** {{< figure src="../media/user-guide/usbx-events/image75.png" title="Device Stack Transfer All Request Abort icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Transfer All Request Abort Event.

**Information Fields** 

- Info Field 1: Endpoint.
- Info Field 2: Completion code.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Device Stack Transfer Request 

#### ux_device_stack_transfer_request

**Icon** {{< figure src="../media/user-guide/usbx-events/image76.png" title="Device Stack Transfer Request icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Device Stack Transfer Request Event.

**Information Fields**

- Info Field 1: Transfer request.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Asix Activate 

#### ux_host_class_asix_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image77.png" title="Host Class Asix Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Asix Activate.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Asix Deactivate 

#### ux_host_class_asix_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image78.png" title="Host Class Asix Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Asix Deactivate Event.

**Information Fields** 

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Asix Interrupt Notification 

#### ux_host_class_asix_interrupt_notification

**Icon** {{< figure src="../media/user-guide/usbx-events/image79.png" title="Host Class Asix Interrupt Notification icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Asix Interrupt Notification Event.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Asix Read 

#### ux_host_class_asix_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image80.png" title="Host Class Asix Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Asix Read Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Asix Write 

#### ux_host_class_asix_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image81.png" title="Host Class Asix Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Asix Write Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Audio Activate 

#### ux_host_class_audio_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image82.png" title="Host Class Audio Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Audio Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Control Value Get 

#### ux_host_class_audio_control_value_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image83.png" title="Host Class Audio Control Value Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Audio Control Value Get Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Control Value Set 

#### ux_host_class_audio_control_value_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image84.png" title="Host Class Audio Control Value Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo I/O driver deferred processing event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Audio control.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Deactivate 

#### ux_host_class_audio_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image85.png" title="Host Class Audio Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Audio Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Read 

#### ux_host_class_audio_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image86.png" title="Host Class Audio Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Audio Read Event.

- Information Fields 
- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Audio Streaming Sampling Get 

#### ux_host_class_audio_streaming_sampling_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image87.png" title="Host Class Audio Streaming Sampling Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Audio Streaming Sampling Get Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Streaming Sampling Set 

#### ux_host_class_audio_streaming_sampling_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image88.png" title="Host Class Audio Streaming Sampling Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Audio Streaming Sampling Set Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Audio Sampling.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Audio Write 

#### ux_host_class_audio_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image89.png" title="Host Class Audio Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Audio Write Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Cdc Acm Activate 

#### ux_host_class_cdc_acm_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image90.png" title="Host Class C D C A C M Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Deactivate 

#### ux_host_class_cdc_acm_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image91.png" title="Host Class C D C A C M Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Abort In Pipe 

#### ux_host_class_cdc_acm_ioctl_abort_in_pipe

**Icon** {{< figure src="../media/user-guide/usbx-events/image92.png" title="Host Class C D C A C M I O C T L Abort In Pipe icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Abort In Pipe Event.

Information Fields 

- Info Field 1: Class instance.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Abort Out Pipe 

#### ux_host_class_cdc_acm_ioctl_abort_out_pipe

**Icon** {{< figure src="../media/user-guide/usbx-events/image93.png" title="[Host Class C D C A C M I O C T L Abort Out Pipe icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Abort Out Pipe Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Get Device Status 

#### ux_host_class_cdc_acm_ioctl_get_device_status

**Icon** {{< figure src="../media/user-guide/usbx-events/image94.png" title="Host Class C D C A C M I O C T L Get Device Status icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Get Device Status Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Device status.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Get Line Coding 

#### ux_host_class_cdc_acm_ioctl_get_line_coding

**Icon** {{< figure src="../media/user-guide/usbx-events/image95.png" title="Host Class C D C A C M I O C T L Get Line Coding icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Get Line Coding Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Notification Callback

#### ux_host_class_cdc_acm_ioctl_notification_callback

**Icon** {{< figure src="../media/user-guide/usbx-events/image96.png" title="Host Class C D C A C M I O C T L Notification Callback icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Notification Callback Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Send Break 

#### ux_host_class_cdc_acm_ioctl_send_break

**Icon** {{< figure src="../media/user-guide/usbx-events/image97.png" title="Host Class C D C A C M I O C T L Send Break icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Send Break Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Set Line Coding 

#### ux_host_class_cdc_acm_ioctl_set_line_coding

**Icon** {{< figure src="../media/user-guide/usbx-events/image97.png" title="The Host Class C D C A C M I O C T L Set Line Coding icon" imgClass="img-responsive center-block" >}}
 icon](../media/user-guide/usbx-events/image98.png)

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Set Line Coding Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Ioctl Set Line State 

#### ux_host_class_cdc_acm_ioctl_set_line_state

**Icon** {{< figure src="../media/user-guide/usbx-events/image99.png" title="Host Class C D C A C M I O C T L Set Line State icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Ioctl Set Line State Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Read 

#### ux_host_class_cdc_acm_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image100.png" title="Host Class C D C A C M Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Read Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested Length.
- Info Field 4: Not used.

### Host Class Cdc Acm Reception Start 

#### ux_host_class_cdc_acm_reception_start

**Icon** {{< figure src="../media/user-guide/usbx-events/image101.png" title="Host Class C D C A C M Reception Start icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Reception Start Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Reception Stop 

#### ux_host_class_cdc_acm_reception_stop

**Icon** {{< figure src="../media/user-guide/usbx-events/image102.png" title="Host Class C D C A C M Reception Stop icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Reception Stop Event.

**Information Fields**

- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Cdc Acm Write 

#### ux_host_class_acm_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image103.png" title="Host Class C D C A C M Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Cdc Acm Write Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested Length.
- Info Field 4: Not used.

### Host Class Dpump Activate 

#### ux_host_class_dpump_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image104.png" title="Host Class Dpump Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Dpump Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Dpump Deactivate 

#### ux_host_class_dpump_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image105.png" title="Host Class Dpump Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Dpump Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Dpump Read 

#### ux_host_class_dpump_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image106.png" title="Host Class Dpump Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Dpump Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Dpump Write 

#### ux_host_class_dpump_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image107.png" title="Host Class Dpump Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Dpump Write Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Hid Activate 

#### ux_host_class_hid_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image108.png" title="Host Class Hid Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Client Register 

#### ux_host_class_hid_client_register

**Icon** {{< figure src="../media/user-guide/usbx-events/image109.png" title="Host Class Hid Client Register icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Client Register Event.

**Information Fields** 

- Info Field 1: Hid client name.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Deactivate 

#### ux_host_class_hid_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image110.png" title="Host Class Hid Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Idle Get 

#### ux_host_class_hid_idle_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image111.png" title="Host Class Hid Idle Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Idle Get Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Idle Set 

#### ux_host_class_hid_idle_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image112.png" title="Host Class Hid Idle Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Idle Set Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Keyboard Activate 

#### ux_host_class_hid_keyboard_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image113.png" title="Host Class Hid Keyboard Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Keyboard Activate Event.

**Information Fields**
<p>Info Field 1: Class instance.
<p>Info Field 2: Hid client instance.
<p>Info Field 3: Not used.
<p>Info Field 4: Not used.

### Host Class Hid Keyboard Deactivate 

#### ux_host_class_hid_keyboard_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image114.png" title="Host Class Hid Keyboard Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Keyboard Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Mouse Activate 

#### ux_host_class_hid_mouse_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image115.png" title="Host Class Hid Mouse Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Mouse Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Mouse Deactivate 

#### ux_host_class_hid_mouse_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image116.png" title="Host Class Hid Mouse Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

- This event represents a USBX Host Class Hid Mouse Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Remote Control Activate 

#### ux_host_class_hid_remote_control_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image117.png" title="Host Class Hid Remote Control Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Remote Control Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Remote Control Deactivate 

#### ux_host_class_hid_remote_control_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image118.png" title="Host Class Hid Remote Control Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Remote Control Deactivate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Hid client instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Report Get 

#### ux_host_class_hid_report_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image119.png" title="Host Class Hid Report Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hid Report Get.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Client report.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hid Report Set 

#### ux_host_class_hid_report_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image120.png" title="Host Class Hid Report Set icon" imgClass="img-responsive center-block" >}}

**Description**
This event represents a USBX Host Class Hid Report Set.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Client report.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hub Activate 

#### ux_host_class_hub_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image121.png" title="Host Class Hub Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hub Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hub Change Detect 

#### ux_host_class_hub_change_detect

**Icon** {{< figure src="../media/user-guide/usbx-events/image122.png" title="Host Class Hub Change Detect icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hub Change Detect Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hub Deactivate 

#### ux_host_class_hub_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image123.png" title="Host Class Hub Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hub Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Hub Port Change Connection Process 

#### ux_host_class_hub_change_connection_process

**Icon** {{< figure src="../media/user-guide/usbx-events/image124.png" title="Host Class Hub Port Change Connection Process icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hub Port Change Connection Process Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Hub Port Change Enable Process 

#### ux_host_class_hub_port_change_enable_process

**Icon** {{< figure src="../media/user-guide/usbx-events/image125.png" title="Host Class Hub Port Change Enable Process icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hub Port Change Enable Process Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Hub Port Change Over Current Process 

#### ux_host_class_hub_port_change_over_current_process

**Icon** {{< figure src="../media/user-guide/usbx-events/image126.png" title="Host Class Hub Port Change Over Current Process icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents allocating a packet via nx_packet_allocate.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Hub Port Change Reset Process 

#### ux_host_class_hub_port_change_reset_process

**Icon** {{< figure src="../media/user-guide/usbx-events/image127.png" title="Host Class Hub Port Change Reset Process icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hub Port Change Reset Process Event.

**Information Fields**

- Info Field 1: Hub. Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Hub Port Change Suspend Process 

#### ux_host_class_hub_port_change_suspend_process

**Icon** {{< figure src="../media/user-guide/usbx-events/image128.png" title="Host Class Hub Port Change Suspend Process icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Hub Port Change Suspend Process Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Port.
- Info Field 3: Port status.
- Info Field 4: Not used.

### Host Class Pima Activate 

#### ux_host_class_pima_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image129.png" title="Host Class Pima Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Deactivate 

#### ux_host_class_pima_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image130.png" title="Host Class Pima Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Device Info Get 

#### ux_host_class_pima_device_info_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image131.png" title="Host Class Pima Device Info Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Device Info Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Pima device.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Device Reset 

#### ux_host_class_pima_device_reset

**Icon** {{< figure src="../media/user-guide/usbx-events/image132.png" title="Host Class Pima Device Reset icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Device Reset Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Notification 

#### ux_host_class_pima_notification

**Icon** {{< figure src="../media/user-guide/usbx-events/image133.png" title="Host Class Pima Notification icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Notification Event.

**Information Fields**

- Info Field 1: Class instance. Info Field 2: Event code.
- Info Field 3: Transaction ID.
- Info Field 4: Parameter1.

### Host Class Pima Number Objects Get 

#### ux_host_class_pima_number_objects_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image134.png" title="Host Class Pima Number Objects Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Number Objects Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Close 

#### ux_host_class_pima_object_close

**Icon** {{< figure src="../media/user-guide/usbx-events/image135.png" title="Host Class Pima Object Close icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Object Close Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Copy 

#### ux_host_class_pima_object_copy

**Icon** {{< figure src="../media/user-guide/usbx-events/image136.png" title="Host Class Pima Object Copy icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Object Copy Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Delete 

#### ux_host_class_pima_object_delete

**Icon** {{< figure src="../media/user-guide/usbx-events/image137.png" title="Host Class Pima Object Delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Object Delete Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Get 

#### ux_host_class_pima_object_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image138.png" title="Host Class Pima Object Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents getting RARP information via nx_rarp_info_get.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Object.
- Info Field 4: Not used.

### Host Class Pima Object Info Get 

#### ux_host_class_pima_object_info_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image139.png" title="Host Class Pima Object Info Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Object Info Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Object.
- Info Field 4: Not used.

### Host Class Pima Object Info Send 

#### ux_host_class_pima_object_info_send

**Icon** {{< figure src="../media/user-guide/usbx-events/image140.png" title="Host Class Pima Object Info Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Object Info Send Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Object Move 

#### ux_host_class_pima_object_move

**Icon** {{< figure src="../media/user-guide/usbx-events/image141.png" title="Host Class Pima Object Move icon" imgClass="img-responsive center-block" >}}

**Description**

This event reprThis event represents a USBX Host Class Pima Object Move Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Object.
- Info Field 4: Not used.

### Host Class Pima Object Send 

#### ux_host_class_pima_object_move

**Icon** {{< figure src="../media/user-guide/usbx-events/image142.png" title="Host Class Pima Object Send icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Object Send Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object.
- Info Field 3: Object buffer.
- Info Field 4: Object length.

### Host Class Pima Object Transfer Abort 

#### ux_host_class_pima_object_transfer_abort

**Icon** {{< figure src="../media/user-guide/usbx-events/image143.png" title="Host Class Pima Object Transfer Abort icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Object Transfer Abort Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Object.
- Info Field 4: Not used.

### Host Class Pima Read 

#### ux_host_class_pima_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image144.png" title="Host Class Pima Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Data length.
- Info Field 4: Not used.

### Host Class Pima Request Cancel 

#### ux_host_class_pima_request_cancel

**Icon** {{< figure src="../media/user-guide/usbx-events/image145.png" title="Host Class Pima Request Cancel icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Request Cancel Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Session Close 

#### ux_host_class_pima_session_close

**Icon** {{< figure src="../media/user-guide/usbx-events/image146.png" title="Host Class Pima Session Close icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Session Close Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Pima session.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Session Open 

#### ux_host_class_pima_session_open

**Icon** {{< figure src="../media/user-guide/usbx-events/image147.png" title="Host Class Pima Session Open icon" imgClass="img-responsive center-block" >}}

**Description**
This event represents a USBX Host Class Pima Session Open Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Pima session.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Storage Ids Get 

#### ux_host_class_pima_session_ids_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image148.png" title="Host Class Pima Storage Ids Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Storage Ids Get Event.

**Information Fields**

- Info Field 1: Class instance.
- nfo Field 2: Storage ID array.
- Info Field 3: Storage ID length.
Info Field 4: Not used.

### Host Class Pima Storage Info Get 

#### ux_host_class_pima_storage_info_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image149.png" title="Host Class Pima Storage Info Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Storage Info Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Storage ID.
- Info Field 3: Storage.
- Info Field 4: Not used.

### Host Class Pima Thumb Get 

#### ux_host_class_pima_thumb_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image150.png" title="Host Class Pima Thumb Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents unaccepting a TCP server connection via nx_tcp_server_socket_unaccept.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Object handle.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Pima Write 

#### ux_host_class_pima_thumb_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image151.png" title="Host Class Pima Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Pima Write.

**Information Fields**

- Info Field 1: Class Instance.
- Info Field 2: Data pointer.
- Info Field 3: Data length.
- Info Field 4: Not used.

### Host Class Printer Activate 

#### ux_host_class_printer_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image152.png" title="Host Class Printer Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Printer Activate Event.

**Information Fields**

- Info Field 1: Pointer to IP instance.
- Info Field 2: Pointer to socket.
- Info Field 3: Type of service.
- Info Field 4: Receive window size.

### Host Class Printer Deactivate 

#### ux_host_class_printer_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image153.png" title="Host Class Printer Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Printer Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Printer Name Get 

#### ux_host_class_printer_name_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image154.png" title="Host Class Printer Name Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Printer Name Get Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Printer Read 

#### ux_host_class_printer_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image155.png" title="Host Class Printer Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Printer Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Printer Soft Reset 

#### ux_host_class_printer_soft_reset

**Icon** {{< figure src="../media/user-guide/usbx-events/image156.png" title="Host Class Printer Soft Reset icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Printer Soft Reset Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Printer Status Get 

#### ux_host_class_printer_status_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image157.png" title="Host Class Printer Status Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Printer Status Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Printer status.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Printer Write 

#### ux_host_class_printer_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image158.png" title="Host Class Printer Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Printer Write.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Prolific Activate 

#### ux_host_class_prolific_activate 

**Icon** {{< figure src="../media/user-guide/usbx-events/image159.png" title="Host Class Prolific Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Activate Event.

**Information Fields** 

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Deactivate 

#### ux_host_class_prolific_deactivate 

**Icon** {{< figure src="../media/user-guide/usbx-events/image160.png" title="Host Class Prolific Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Abort In Pipe 

#### ux_host_class_prolific_ioctl_abort_in_pipe

**Icon** {{< figure src="../media/user-guide/usbx-events/image161.png" title="Host Class Prolific I O C T L Abort In Pipe icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Ioctl Abort In Pipe Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Abort Out Pipe 

#### ux_host_class_prolific_ioctl_abort_out_pipe

**Icon** {{< figure src="../media/user-guide/usbx-events/image162.png" title="Host Class Prolific I O C T L Abort Out Pipe icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Ioctl Abort Out Pipe Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Get Device Status 

#### ux_host_class_prolific_ioctl_get_device_status

**Icon** {{< figure src="../media/user-guide/usbx-events/image163.png" title="Host Class Prolific I O C T L Get Device Status icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Ioctl Get Device Status Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Device status.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Get Line Coding 

#### ux_host_class_prolific_ioctl_get_line_coding

**Icon** {{< figure src="../media/user-guide/usbx-events/image164.png" title="Host Class Prolific I O C T L Get Line Coding icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Ioctl Get Line Coding Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Purge 

#### ux_host_class_prolific_ioctl_purge

**Icon** {{< figure src="../media/user-guide/usbx-events/image165.png" title="Host Class Prolific Ioctl Purge icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Ioctl Purge Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Report Device 

#### ux_host_class_prolific_ioctl_report_device

**Icon** {{< figure src="../media/user-guide/usbx-events/image166.png" title="Host Class Prolific I O C T L Report Device icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Ioctl Report Device Status Change Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Send Break 

#### ux_host_class_prolific_ioctl_send_break

**Icon** {{< figure src="../media/user-guide/usbx-events/image167.png" title="Host Class Prolific I O C T L Send Break icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Ioctl Send Break Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Set Line Coding 

#### ux_host_class_prolific_ioctl_set_line_coding

**Icon** {{< figure src="../media/user-guide/usbx-events/image168.png" title="Host Class Prolific I O C T L Set Line Coding icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Ioctl Set Line Coding Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Ioctl Set Line State 

#### ux_host_class_prolific_ioctl_set_line_state

**Icon** {{< figure src="../media/user-guide/usbx-events/image169.png" title="Host Class Prolific I O C T L Set Line State icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Ioctl Set Line State Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Parameter.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Read 

#### ux_host_class_prolific_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image170.png" title="Host Class Prolific Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Prolific Reception Start 

#### ux_host_class_prolific_reception_start

**Icon** {{< figure src="../media/user-guide/usbx-events/image171.png" title="Host Class Prolific Reception Start icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Reception Start Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Reception Stop 

#### ux_host_class_prolific_reception_stop

**Icon** {{< figure src="../media/user-guide/usbx-events/image172.png" title="Host Class Prolific Reception Stop icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Reception Stop Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Prolific Write 

#### ux_host_class_prolific_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image173.png" title="Host Class Prolific Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Prolific Write Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Data pointer.
- Info Field 3: Requested length.
- Info Field 4: Not used.

### Host Class Storage Activate 

#### ux_host_class_storage_activate

**Icon** {{< figure src="../media/user-guide/usbx-events/image174.png" title="Host Class Storage Activate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Activate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Deactivate 

#### ux_host_class_storage_deactivate

**Icon** {{< figure src="../media/user-guide/usbx-events/image175.png" title="Host Class Storage Deactivate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Deactivate Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Media Capacity Get 

#### ux_host_class_storage_media_capacity_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image176.png" title="Host Class Storage Media Capacity Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Media Capacity Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Media Format Capacity Get

#### ux_host_class_storage_media_format_capacity_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image177.png" title="Host Class Storage Media Format Capacity Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Media Format Capacity Get Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

#### Host Class Storage Media Mount 

#### ux_host_class_storage_media_mount

**Icon** {{< figure src="../media/user-guide/usbx-events/image178.png" title="Host Class Storage Media Mount icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Media Mount Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Sector.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Media Open 

#### ux_host_class_storage_media_open

**Icon** {{< figure src="../media/user-guide/usbx-events/image179.png" title="Host Class Storage Media Open icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Media Open Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Media.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Media Read 

#### ux_host_class_storage_media_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image180.png" title="Host Class Storage Media Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Media Read Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Sector start.
- Info Field 3: Sector count.
- Info Field 4: Data pointer.

### Host Class Storage Media Write 

#### ux_host_class_storage_media_write

**Icon** {{< figure src="../media/user-guide/usbx-events/image181.png" title="Host Class Storage Media Write icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Media Write Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Sector start.
- Info Field 3: Sector count.
- Info Field 4: Data pointer.

### Host Class Storage Request Sense 

#### ux_host_class_storage_request_sense

**Icon** {{< figure src="../media/user-guide/usbx-events/image182.png" title="Host Class Storage Request Sense icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Request Sense Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Start Stop 

#### ux_host_class_storage_start_stop

**Icon** {{< figure src="../media/user-guide/usbx-events/image183.png" title="Host Class Storage Start Stop icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Start Stop Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Start stop signal.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Class Storage Unit Ready Test 

#### ux_host_class_storage_unit_ready_test

**Icon** {{< figure src="../media/user-guide/usbx-events/image184.png" title="Host Class Storage Unit Ready Test icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Class Storage Unit Ready Test Event.

**Information Fields**

- Info Field 1: Class instance.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Class Instance Create 

#### ux_host_class_instance_create

**Icon** {{< figure src="../media/user-guide/usbx-events/image185.png" title="Host Stack Class Instance Create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Class Instance Create Event.

**Information Fields**

- Info Field 1: Class.
- Info Field 2: Class Instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Class Instance Destroy 

#### ux_host_class_instance_create

**Icon** {{< figure src="../media/user-guide/usbx-events/image186.png" title="Host Stack Class Instance Destroy icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Class Instance Destroy Event.

**Information Fields**

- Info Field 1: Class.
- Info Field 2: Class Instance.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Configuration Delete 

#### ux_host_class_configuration_delete

**Icon** {{< figure src="../media/user-guide/usbx-events/image187.png" title="Host Stack Configuration Delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Configuration Delete Event.

**Information Fields**

- Info Field 1: Configuration.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Configuration Enumerate 

#### ux_host_stack_configuration_enumerate

**Icon** {{< figure src="../media/user-guide/usbx-events/image188.png" title="Host Stack Configuration Enumerate icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Configuration Enumerate Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Stack Configuration Instance Create 

#### ux_host_stack_configuration_instance_create

**Icon** {{< figure src="../media/user-guide/usbx-events/image189.png" title="Stack Configuration Instance Create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Configuration Instance Create Event.

**Information Fields**

- Info Field 1: Configuration.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Configuration Instance Delete 

#### ux_host_stack_configuration_instance_delete

**Icon** {{< figure src="../media/user-guide/usbx-events/image190.png" title="Host Stack Configuration Instance Delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Configuration Instance Delete Event.

**Information Fields**

- Info Field 1: Configuration.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Configuration Set 

#### ux_host_stack_configuration_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image191.png" title="Host Stack Configuration Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Configuration Set Event.

**Information Fields**

- Info Field 1: Configuration.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Address Set 

#### ux_host_stack_device_address_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image192.png" title="Host Stack Device Address Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Device Address Set Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Device Address.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Configuration Get 

#### ux_host_stack_device_configuration_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image193.png" title="Host Stack Device Configuration Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Device Configuration Get Event.

**Information Fields**

- Info Field 1: Device.
- nfo Field 2: Configuration.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Configuration Select 

#### ux_host_stack_device_configuration_select

**Icon** {{< figure src="../media/user-guide/usbx-events/image194.png" title="Host Stack Device Configuration Select icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Device Configuration Select Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Configuration.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Descriptor Read 

#### ux_host_stack_device_descriptor_read

**Icon** {{< figure src="../media/user-guide/usbx-events/image195.png" title="Host Stack Device Descriptor Read icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Device Descriptor Read Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Get 

#### ux_host_stack_device_get

**Icon** {{< figure src="../media/user-guide/usbx-events/image196.png" title="Host Stack Device Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Device Get Event.

**Information Fields**

- Info Field 1: Device index.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Device Remove 

#### ux_host_stack_device_remove

**Icon** {{< figure src="../media/user-guide/usbx-events/image197.png" title="Host Stack Device Remove icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Device Remove Event.

**Information Fields**

- Info Field 1: Hcd.
- Info Field 2: Parent.
- Info Field 3: Port Index.
- Info Field 4: Device.

### Host Stack Device Resource Free 

#### ux_host_stack_device_resource_free

**Icon** {{< figure src="../media/user-guide/usbx-events/image198.png" title="Host Stack Device Resource Free icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Device Resource Free Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Endpoint Instance Create 

#### ux_host_stack_endpoint_instance_create

**Icon** {{< figure src="../media/user-guide/usbx-events/image199.png" title="Host Stack Endpoint Instance Create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Endpoint Instance Create Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Endpoint Instance Delete 

#### ux_host_stack_endpoint_instance_delete

**Icon** {{< figure src="../media/user-guide/usbx-events/image200.png" title="Host Stack Endpoint Instance Delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Endpoint Instance Delete Event.

**Information Fields**

- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Endpoint Reset 

#### ux_host_stack_endpoint_reset

**Icon** {{< figure src="../media/user-guide/usbx-events/image201.png" title="Host Stack Endpoint Reset icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Endpoint Reset Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Endpoint Transfer Abort 

#### ux_host_stack_endpoint_transfer_abort

**Icon** {{< figure src="../media/user-guide/usbx-events/image202.png" title="Host Stack Endpoint Transfer Abort icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Endpoint Transfer Abort Event.

**Information Fields**

- Info Field 1: Endpoint.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Host Controller Register 

#### ux_host_stack_hcd_register

**Icon** {{< figure src="../media/user-guide/usbx-events/image203.png" title="Host Stack Host Controller Register icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Host Controller Register.

**Information Fields**

- Info Field 1: Hcd Name.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Initialize 

#### ux_host_stack_initialize

**Icon** {{< figure src="../media/user-guide/usbx-events/image204.png" title="Host Stack Initialize icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Initialize Event.

**Information Fields**

- Info Field 1: Not used.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Endpoint Get 

#### Interface_ TCP retry entry

**Icon** {{< figure src="../media/user-guide/usbx-events/image205.png" title="Host Stack Interface Endpoint Get icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents an internal NetX Duo TCP retry event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Endpoint index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Instance Create 

#### ux_host_stack_interface_instance_create

**Icon** {{< figure src="../media/user-guide/usbx-events/image206.png" title="Host Stack Interface Instance Create icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Interface Instance Create Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Instance Delete 

#### ux_host_stack_interface_instance_delete

**Icon** {{< figure src="../media/user-guide/usbx-events/image207.png" title="Host Stack Interface Instance Delete icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Interface Instance Delete Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Set 

#### ux_host_stack_interface_set

**Icon** {{< figure src="../media/user-guide/usbx-events/image208.png" title="Host Stack Interface Set icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Interface Set Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Interface Setting Select 

#### ux_host_stack_interface_setting_select

**Icon** {{< figure src="../media/user-guide/usbx-events/image209.png" title="Host Stack Interface Setting Select icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Interface Setting Select Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack New Configuration Create 

#### ux_host_stack_new_configuration_create

**Icon** {{< figure src="../media/user-guide/usbx-events/image210.png" title="Host Stack New Configuration Create icon" imgClass="img-responsive center-block" >}}

**Description**
 
This event represents a USBX Host Stack New Configuration Create Event.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Configuration.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack New Device Create 

#### ux_host_stack_new_device_create

**Icon** {{< figure src="../media/user-guide/usbx-events/image211.png" title="Host Stack New Device Create icon" imgClass="img-responsive center-block" >}}

**Description**
 
 This event represents a USBX Host Stack New Device Create Event.

**Information Fields**

- Info Field 1: Hcd.
- Info Field 2: Device owner.
- Info Field 3: Port index.
- Info Field 4: Device.

### Host Stack New Endpoint Create 

#### ux_host_stack_new_endpoint_create

**Icon** {{< figure src="../media/user-guide/usbx-events/image212.png" title="Host Stack New Endpoint Create icon" imgClass="img-responsive center-block" >}}

**Description**
 
This event represents a USBX Host Stack New Endpoint Create Event.

**Information Fields**

- Info Field 1: Interface.
- Info Field 2: Endpoint.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Root Hub Change Process 

#### ux_host_stack_rh_change_process

**Icon** {{< figure src="../media/user-guide/usbx-events/image213.png" title="Host Stack Root Hub Change Process icon" imgClass="img-responsive center-block" >}}

**Description**
 
This event represents a USBX Host Stack Root Hub Change Process.

**Information Fields**

- Info Field 1: Port index.
- Info Field 2: Not used.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Root Hub Device Extraction 

#### ux_host_stack_rh_device_extraction

**Icon** {{< figure src="../media/user-guide/usbx-events/image214.png" title="Host Stack Root Hub Device Extraction icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Root Hub Device Extraction Event.

**Information Fields**

- Info Field 1: Hcd.
- Info Field 2: Port index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Root Hub Device Insertion 

#### ux_host_stack_rh_device_insertion

**Icon** {{< figure src="../media/user-guide/usbx-events/image215.png" title="Host Stack Root Hub Device Insertion icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Root Hub Device Insertion.

**Information Fields**

- Info Field 1: Hcd.
- Info Field 2: Port index.
- Info Field 3: Not used.
- Info Field 4: Not used.

### Host Stack Transfer Request 

#### ux_host_stack_transfer_request

**Icon** {{< figure src="../media/user-guide/usbx-events/image216.png" title="Host Stack Transfer Request icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Transfer Request.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Endpoint.
- Info Field 3: Transfer request.
- Info Field 4: Not used.

### Host Stack Transfer Request Abort 

#### Internal I/O driver get status

**Icon** {{< figure src="../media/user-guide/usbx-events/image217.png" title="Host Stack Transfer Request Abort icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Host Stack Transfer Request Abort.

**Information Fields**

- Info Field 1: Device.
- Info Field 2: Endpoint.
- Info Field 3: Transfer request.
- Info Field 4: Not used.

### USBX Error 

#### ux_error

**Icon** {{< figure src="../media/user-guide/usbx-events/image218.png" title="U S B X Error icon" imgClass="img-responsive center-block" >}}

**Description**

This event represents a USBX Error Event.

**Information Fields**

- Info Field 1: USBX error.
- Info Field 2: Error Name.
- Info Field 3: Not used.
- Info Field 4: Not used.