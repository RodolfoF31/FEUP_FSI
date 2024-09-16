# CTF Semana #12 - (Find-my-TLS) Report

For this week's Capture The Flag (CTF) challenge, our objective was to scrutinize a pcap file using Wireshark, unraveling the intricacies of a TLS connection. Our investigation commenced with the file's exploration, employing a filter based on the specified hex value. This unique value, `52362c11ff0ea3a000e1b48dc2d99e04c6d06ea1a061d5b8ddbf87b001745a27`, served as our guide to isolate the TLS handshake requiring scrutiny.

To accomplish this, we utilized the search functionality (CTRL+F) and opted for the hex option, seamlessly pasting the provided hex value.

**Filtered TLS Packets:**
![Filtered TLS](/images/filtered_tls.png)

Now equipped with the relevant packets, exclusively pertaining to TLSv1.2, we proceeded to dissect the handshake process.

In accordance with the provided flag structure:

- `<frame_start>` and `<frame_end>` represent the first and last frame numbers, respectively, corresponding to the TLS handshake procedure.
- `<selected_cipher_suite>` denotes the chosen ciphersuite for the TLS connection (in name, not code).
- `<total_encrypted_appdata_exchanged>` signifies the cumulative size of encrypted data exchanged in this channel until termination.
- `<size_of_encrypted_message>` specifies the size of the encrypted message in the handshake that concludes the handshake procedure.

Our analysis revealed that `<frame_start>` is 814, and `<frame_end>` is 819. The ciphersuite, `<selected_cipher_suite>`, was identified in Frame 816 (Server Hello) as `TLS_RSA_WITH_AES_128_CBC_SHA256`.

**Selected Cipher:**
![Selected Cipher](/images/selected_cipher.png)

Remaining were the values for `<total_encrypted_appdata_exchanged>` and `<size_of_encrypted_message>`. Calculating `<total_encrypted_appdata_exchanged>` involved summing the lengths of the encrypted data in the Application Data frames.

**Application Data Analysis:**
![AppData Analysis](/images/app_data.png)

The resulting total for `<total_encrypted_appdata_exchanged>` was 1264. For `<size_of_encrypted_message>`, we examined the Encrypted Handshake Message in Frame 818.

**Encrypted Handshake Message:**
![Encrypted Message](/images/encrypted_message.png)

Thus, our conclusive findings led to the construction of the flag:

`flag{814-819-TLS_RSA_WITH_AES_128_CBC_SHA256-1264-80}`
