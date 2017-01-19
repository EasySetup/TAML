// statement.proto
//
// Definition of statements for The Attribute Management Language (TAML)
//

message Key {
   optional string csi = 1;
   optional string publicKey = 2;
}
message Attribute {
  optional string group = 1;
  optional string networkRole = 2;
  optional string friendlyName = 3;
}
message While {
  optional string afterTime = 1;   // should this be string or binary time?
  optional string beforeTime = 2;
  optional string locationRange = 3;
}
message Has {
  repeated Attribute attribute = 1;
}
message Attest {
  required string says = 1; // UAID string for speaking public key
  required string this = 2; // UAID string for target of attestation/assignment
  message Has {
    repeated Attribute attribute = 1;
  }
  required Has has = 3;     // list of attributes
  optional While while = 4;
  optional string signed = 5; // opaque string using signature algorithm of
                              // speaking public key (from csi)
}
message AttributeRange {
  optional string groupRange = 1;
  optional string networkRoleRange = 2
  optional string friendlyNameRange = 3;
}
message Delegate {
  required string says = 1;      // UAID octets for speaking public key
  required string this = 2;      // UAID octets for target of delegation
  optional string speaksFor = 3; // UAID of root, speaker assumed if not included
  message SpeaksAbout {
    repeated AttributeRange attributerange = 1;
  }
  required SpeaksAbout speaksabout = 6;  // list of attribute ranges
  optional While while = 4;
  optional string signed = 5;  // opaque string of digital signature of
                               // speaking public key
}
message Represent {
  required string says = 1; // UAID string for speaking public key
  required string as = 6;   // UAID of represented key
  required string this = 2; // UAID string for target of attestation/assignment
  required Has has = 3;     // list of attributes
  optional While while = 4;
  optional string signed = 5; // opaque string using signature algorithm of
                              // speaking public key (from key’s csi)
}
message Statement {
  optional Key key = 1;
  optional Attest attest = 2;
  optional Delegate delegate = 3;
  optional Represent represent = 4;
}

