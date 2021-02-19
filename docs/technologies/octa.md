# OCTA protocol

OCTA Connect is a hardware platform for low-power and wireless projects. OCTA is part of the IDLab IoT platform together with DYAMAND. To communicate with DYAMAND, the OCTA protocol is being designed.

## Tests for OCTA protocol

!!! success "octa.0.0"
     Any valid OCTA message without payload should be parseable
!!! success "octa.0.1"
     All added headers should be available in the parsed message
!!! success "octa.0.2"
     Any valid OCTA message with payload should be parseable
!!! success "octa.0.3"
     The value for all headers when serialized should be the same byte array as the generated one
