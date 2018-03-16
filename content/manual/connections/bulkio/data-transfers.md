---
title: "Data Transfers"
weight: 10
---

Data transfers happen through the `pushPacket()` method call of a REDHAWK component’s port object. This method transfers the data from the uses-side port to the corresponding connected provides-side port. The data is marshaled by the middleware (omniORB) and placed on a queue for processing by the receiving component. The implementations of the `pushPacket()` methods are maximized for the efficiency of data throughput while providing network-accessible ingest/egress of data and minimizing the complexity of the implementation.

##### BulkIO Data Flow via `pushPacket()`
![bulkio Data Flow via pushPacket()](../../images/REDHAWK_bulkio_dataflow.png)

Each implementation maintains the required behavior of providing an SRI object before receiving any data transfers. This is accomplished by calling the `pushSRI()` method of the port with an SRI object. In most cases, a component takes the ingest SRI object received from an input port, makes any required modifications as necessary, and passes this object down stream over its output port. If a component does not provide an SRI object before its first `pushPacket()`, the port creates a default SRI object with nominal values to pass out the port.

The following sections explain the different methods for transferring supported data types by a component.

{{% notice warning %}}
For the current implementation of omniORB, the `/etc/omniORB.cfg` maintains the configurable maximum transfer size defined by the value for giopMaxMsgSize. The default maximum transfer size is set to `2097152` (2 MB). For every `pushPacket()`, the data+headers must be less than this value; otherwise, a `MARSHAL` exception is raised by the middleware. This maximum value can be found during run time by using the `omniORB::giopMaxMsgSize()` function call or the `bulkio::Const::MAX_TRANSFER_BYTES` value.
{{% /notice %}}

### Vector Data
A component usually ingests and egresses data from its ports in the service function. A component with a provides-port (input), grabs data from the port using the `getPacket()` method. This method returns a `dataTransfer` object (described in [DataTransfer Member Descriptions](#datatransfer-member-descriptions)) from the input port’s data queue or a null/None value if the queue is empty.

The following code snippet is an example of the `getPacket()` method.

```c++
/**
   Grab data from the port's getPacket method
 */
bulkio::InFloatPort::dataTransfer *pkt;
pkt = inFloatPort->getPacket( bulkio::Const::NON_BLOCKING );

// check if a valid packet was returned
if ( pkt == NULL ) {
  return NOOP;
}

// check if any SRI changes occurred
if ( pkt->sriChanged ) {
  outFloatPort->pushSRI( pkt->SRI );
}

...   perform some algorithm on the data:  pkt->dataBuffer ...
```

##### DataTransfer Member Descriptions
| **Name**          | **Type**            | **Description**                                                                                                                                                                                                                  |
| :---------------- | :------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dataBuffer        | std::vector\<TYPE\>   | The data transferred, where TYPE is some native CORBA type (e.g., float, short). The sequence may be zero-length.                                                                                                                |
| T                 | PrecisionUTCTime    | The epoch birth date of the first sample of the sequence.                                                                                                                                                                        |
| EOS               | boolean             | Flag describing whether this particular stream ends with this buffer of data.                                                                                                                                                    |
| streamID          | string              | Stream ID for this particular payload. This value is used to reconcile this sequence of data with a particular Stream SRI data structure.                                                                                        |
| SRI               | `BULKIO::StreamSRI` | The Signal Related Information (metadata) associated with the buffer of data.                                                                                                                                                    |
| sriChanged        | boolean             | Flag that describes if a new SRI object was received for this stream.                                                                                                                                                            |
| inputQueueFlushed | boolean             | Flag that signifies if the port’s incoming data queue was flushed (purged) because the limit was reached. This happens when the consuming component does not keep up with the incoming rate at which the data is being received. |

The following code snippet is an example of the `pushPacket()` method call for vector data with sample parameters. The `pushPacket()` parameters for vector data are described in the table below.

```c++
std::vector<short> data;

...  perform algorithm and save results to data vector ...

BULKIO::PrecisionUTCTime tstamp = bulkio::time::utils::now();
outShortPort->pushPacket( data, tstamp, false, "sample" );
```
##### `pushPacket()` Parameter Descriptions for Vector Data
| **Name** | **Type**         | **Description**                                                                                                                            |
| :------- | :--------------- | :----------------------------------------------------------------------------------------------------------------------------------------- |
| data     | sequence\<TYPE\> | A sequence of a particular type, where TYPE is some native CORBA type (e.g., float, short). The sequence may be zero-length.               |
| T        | PrecisionUTCTime | The epoch birth date of the first sample of the sequence passed in this call.                                                              |
| EOS      | boolean          | Flag describing whether this particular stream ends with this `pushPacket()` call.                                                         |
| streamID | string           | Stream ID for this particular payload. This string is used to reconcile this sequence of data with a particular Stream SRI data structure. |

### String Data/XML Document

The following code snippet is an example of the `pushPacket()` method call for string data with sample parameters. The `pushPacket()` parameters for string data are described in the table below.

```c++
std::string data;

... generate some text data to transfer ...

outStringPort->pushPacket( data.c_str(), false, "sample" );
```

##### `pushPacket()` Parameter Descriptions for String Data
| **Name** | **Type** | **Description**                                                                                                                            |
| :------- | :------- | :----------------------------------------------------------------------------------------------------------------------------------------- |
| data     | `char`     | A string of characters to pass between components. Also used for passing XML documents in-line between components.                         |
| EOS      | `boolean`  | Flag describing whether this particular stream ends with this `pushPacket()` call.                                                         |
| streamID | `string`   | Stream ID for this particular payload. This string is used to reconcile this sequence of data with a particular Stream SRI data structure. |

### URL/File Data

The following code snippet is an example of the `pushPacket()` method call for file transfers with sample parameters. The `pushPacket()` parameters for file transfers are described in the table below.

```c++
std::string uri;

uri = "file:///data/samples.8t";

... open the file, fill with samples of data, close the file ...

BULKIO::PrecisionUTCTime tstamp = bulkio::time::utils::now();
outURLPort->pushPacket( uri.c_str(), tstamp, false, "sample" );
```
##### `pushPacket()` Parameter Descriptions for File Transfers
| **Name** | **Type**         | **Description**                                                                                                                                                                              |
| :------- | :--------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| url      | `string`           | The URL describing the file. Appropriate schemes for the URLs are file:// and sca://. The file scheme applies to the local file system, while the SCA scheme applies to the SCA File Manager. |
| T        | `PrecisionUTCTime` | An appropriate time stamp for the data being passed.                                                                                                                                         |
| EOS      | `boolean`          | Flag describing whether this particular stream ends with this `pushPacket()` call.                                                                                                           |
| streamID | `string`           | Stream ID for this particular payload. This string is used to reconcile this sequence of data with a particular Stream SRI data structure.                                                   |

Data files may be sent via the BulkIO `dataFile` type. When using the BulkIO `dataFile` type, a filename is passed to the `pushPacket()` method. The location of the file is specified by a URI that either points to the local file system or the SDR file system. To support portability, use of the SDR file system is recommended. The table below describes the URI options in greater detail.

#### URI Options
| **Protocol** | **Format**                  | **Description**                                                                                                                                                                                                           |
| :----------- | :-------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| file         | `file://[localhost]/<path>` | A host specific absolute path of the deployed component/device/service.                                                                                                                                                   |
| sdr          | `sdr://[ior]/<path>`        | A path on the Domain Manager’s file system. If the optional IOR is provided, this path provides the reference to the Domain Manager. If not provided, the Domain Manager is the default used by the component/device/service |

### SDDS Stream Definition

The SDDS Stream Definition object defines a multicast connection source for data from a network interface. The methods for the SDDS Stream Definition Interface do not follow the normal BulkIO `pushPacket()` convention. Instead this interface defines `attach()` and `detach()` methods as described in the code snippet and tables below.

```c++
/**
 *  SDDS Stream Definition Interface
 */

/**
  *  attach : request to an attachment to a specified network data source
  */
char  *attach( BULKIO::SDDSStreamDefinition stream, const char * userid );

/**
  * detach: unlatch from a network data source
  */
void  detach( const char* attachId );
```

##### `char* attach()`
| **Name**     | **Type**             | **Description**                                             |
| :----------- | :------------------- | :---------------------------------------------------------- |
| return value | char\*               | Attachment identifier assigned to this request.             |
| stream       | SDDSStreamDefinition | Stream definition object describing a multicast address ([SDDS Stream Definition Member Descriptions](#sdds-stream-definition-member-descriptions)). |
| userid       | const char\*         | Identification for the request of the attach call.          |

##### `void detach()`
| **Name**     | **Type**             | **Description**                                             |
| :----------- | :------------------- | :---------------------------------------------------------- |
| attachId     | char\*               | Attachment identifier returned from attach request.         |

##### SDDS Stream Definition Member Descriptions
| **Name**         | **Type**        | **Description**                                             |
| :--------------- | :-------------- | :---------------------------------------------------------- |
| ID               | string          | Unique identifier for the source stream.                    |
| dataFormat       | SDDSDataDigraph | Data format of the data stream samples.                     |
| multicastAddress | string          | IPv4 network address in dot notation form.                  |
| vlan             | unsigned long   | Virtual lan identifier as defined by 802.11q.               |
| port             | unsigned long   | IP port number associated with the network connection.      |
| sampleRate       | unsigned long   | Expected sample rate for the data.                          |
| timeTagValid     | boolean         | Denotes if data stream can provide valid time stamp values. |
| privateInfo      | string          | Allows for user-defined values to be passed as a string.    |
