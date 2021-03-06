## Business case

At the conclusion of the ASP project in April 2019, it is anticipated that STTP will be a well-tested, thoroughly vetted, production-grade protocol that will be supported by ASP project team vendors.  An open source tool suite for STTP will be developed as part of the project (see [Appendix B](APIReference.md)) that will include a test harness that will allow utilities and vendors outside the project to test and validate STTP in their systems and API's.

STTP offers both short-term cost savings and strategic value in that it is:

#### Intrinsically Robust

By design, STTP packet sizes are small and are optimized for network MTU size reducing fragmentation which results in more efficient TCP performance and less overall data loss with UDP.  STTP also puts significantly less stress on network routing equipment and facilitates mixing of streaming data traffic and other general network communications.  With STTP, purpose built networks are not required to reliably support very large phasor data streams.

#### Security Centric

STTP has been built using a "security first" design approach.  Authentication to establish a connection with other parties requires a certificate.  While public certificate providers can be used, it is recommended that symmetric certificates be exchanged out-of-band to avoid the risk and cost of management of public keys. Best-practice encryption is natively available in STTP but not required given the common practice to manage encryption at the network layer.

#### Reduces First Cost

A protocol similar to STTP called GEP has been measured <sup>[[5](References.md#user-content-ref5)]</sup> to have less than half the band width requirements of IEEE C37.118 <sup>[[1](References.md#user-content-ref1)]</sup> when used with TCP and simple methods for lossless compression.  With the compression, a single signal or measurement point (i.e., an identifier, timestamp, value and quality code) requires only 2.5 bytes. By comparison, IEEE C37.118 requires 4.5 bytes per measurement on average. The signal-based GEP protocol incorporates Pub/Sub data exchange methods so that unnecessary data points need not be exchanged - thereby further reducing overall bandwidth requirements as compared to IEEE C37.118.

#### Reduces Operating Cost

STTP will automatically exchange and synchronize measurement level meta-data using a GUID as the key value to allow the self-initialization and integration of rich meta-data with points from multiple connected synchrophasor networks.  This eliminates the need to map measurements to a pre-defined set identifiers and dispenses with the cost and hassles of synchronization of individual utility configuration with a centralized registry. Permissions for data subscriptions can be grouped and filtered using expressions to assure that only the signals that are authorized are shared (for example, all phasors from a specified substation) while the set of points available is dynamically adjusted as PMUs come and go without the need for point-by-point administrator approval.

#### An Enabling Technology

STTP provides an alternative to the existing method for utility data exchange that will enable future generations of SCADA/EMS systems to both (1) utilize full-resolution synchrophasor data streams and (2) significantly reduce the cost of maintaining the configuration of components to exchange other real-time data.  An ISO/RTO will typically exchange hundreds of thousands of data points every few seconds with its members and neighbors.  

> :information_source: ICCP (IEC 60870-6/TASE.2) is the international standard used to exchange "real-time" SCADA data among electric utilities.  Analog measurement data is typically exchanged continuously every 2 to 10 seconds with bi-modal data such as breaker status information only being exchanged "on change".  ICCP came into coordinated use in North America in the mid-1990s.

Promising technologies are being developed for cloud computing and these technologies are moving toward native implementations at individual utilities and ISOs.  These cloud computing technologies can also be leveraged to support larger native implementations such as those for an interconnect.  The common theme among these technologies is the ability to process significantly more data quickly with improved reliability.

It's possible that a protocol like STTP which allows secure, low-latency, high-volume data exchange among utilities at low cost can be a major factor in driving change toward these new technologies. New higher-speed forms of inter-utility interaction will be possible, and new approaches for providing utility information services will be realizable.  


#### Built Upon A Proven Approach

 STTP will enhance the successful design elements of the Gateway Exchange Protocol (GEP) as a foundation and improve upon it. GEP is currently in production use by Dominion, Entergy, MISO, PeakRC, TVA, FP&L, Southern Company, among others.
