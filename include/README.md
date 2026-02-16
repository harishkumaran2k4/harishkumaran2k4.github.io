# ECDSA NIST P-256 Implementation

This directory contains the ECDSA (Elliptic Curve Digital Signature Algorithm) implementation for the NIST P-256 curve, sourced from the [Xilinx Vitis Security Library](https://github.com/Xilinx/Vitis_Libraries/tree/main/security).

## Contents

### Header Files

- **`xf_security/ecdsa_nistp256.hpp`**: Main ECDSA implementation for NIST P-256 curve
  - Signing and verification functions
  - Elliptic curve point operations
  - Optimized for FPGA synthesis using HLS pragmas
  
- **`xf_security/modular.hpp`**: Modular arithmetic operations
  - Montgomery multiplication
  - Modular inverse
  - Modular addition, subtraction, and multiplication

## About ECDSA NIST P-256

ECDSA (Elliptic Curve Digital Signature Algorithm) is a widely-used digital signature algorithm based on elliptic curve cryptography. The NIST P-256 curve (also known as secp256r1 or prime256v1) is one of the most commonly used elliptic curves for cryptographic applications.

### Key Features

- **256-bit security**: Provides equivalent security to RSA-3072
- **Efficient operations**: Optimized elliptic curve arithmetic
- **Hardware acceleration**: Designed for FPGA implementation
- **Pre-computed tables**: Uses pre-computed point tables for faster scalar multiplication

## Algorithm Overview

### Curve Parameters (NIST P-256)

The implementation uses the following NIST P-256 curve parameters:

- **Prime field**: p = 2^256 - 2^224 + 2^192 + 2^96 - 1
- **Curve equation**: y² = x³ - 3x + b
- **Base point**: G = (Gx, Gy)
- **Order**: n (the order of the base point G)

### Main Functions

#### Signing (`nistp256Sign`)

```cpp
bool nistp256Sign(ap_uint<256> hash, ap_uint<256> k, ap_uint<256> privateKey, 
                  ap_uint<256>& r, ap_uint<256>& s)
```

- **Input**: 
  - `hash`: Message digest (256-bit)
  - `k`: Random nonce (must be unique for each signature)
  - `privateKey`: Private signing key
- **Output**: 
  - `r`, `s`: Signature pair
- **Returns**: `true` if signature is valid, `false` otherwise

#### Verification (`nistp256Verify`)

```cpp
bool nistp256Verify(ap_uint<256> r, ap_uint<256> s, ap_uint<256> hash, 
                    ap_uint<256> Px, ap_uint<256> Py)
```

- **Input**: 
  - `r`, `s`: Signature pair
  - `hash`: Message digest (256-bit)
  - `Px`, `Py`: Public key coordinates
- **Returns**: `true` if signature is valid, `false` otherwise

## Implementation Details

### Optimizations

1. **NAF (Non-Adjacent Form)**: Uses NAF representation for efficient scalar multiplication
2. **Jacobian Coordinates**: Uses Jacobian coordinate system to avoid expensive field inversions
3. **Pre-computed Tables**: 64 pre-computed points for faster base point multiplication
4. **Fast Reduction**: Specialized modular reduction for the P-256 prime field

### HLS Pragmas

The code includes Xilinx HLS (High-Level Synthesis) pragmas for hardware optimization:
- `#pragma HLS inline`: For function inlining
- `#pragma HLS pipeline off`: To control pipelining
- `#pragma HLS bind_storage`: For memory resource mapping
- `#pragma HLS allocation`: For resource allocation control

## Dependencies

This implementation requires:
- **Xilinx Vivado HLS**: For FPGA synthesis
- **ap_int.h**: Arbitrary precision integer types
- **hls_stream.h**: HLS streaming interfaces (for modular.hpp)

## License

Copyright 2019 Xilinx, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## References

- [NIST P-256 Curve Specification](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf)
- [Xilinx Vitis Security Library](https://github.com/Xilinx/Vitis_Libraries/tree/main/security)
- [ECDSA Algorithm](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)

## Security Considerations

⚠️ **Important Security Notes**:

1. **Random Nonce**: The parameter `k` in signing must be:
   - Cryptographically random
   - Unique for every signature
   - Never reused (reusing k with the same private key compromises the private key)

2. **Side-Channel Attacks**: This implementation may be vulnerable to timing and power analysis attacks. Additional countermeasures may be needed for production use.

3. **Key Management**: Ensure proper key generation and storage practices.

4. **Hash Function**: Always use a cryptographically secure hash function (SHA-256 or better) before signing.

## Usage Example (Conceptual)

```cpp
#include "xf_security/ecdsa_nistp256.hpp"

// Generate signature
ap_uint<256> messageHash = /* SHA-256 hash of message */;
ap_uint<256> privateKey = /* Private key */;
ap_uint<256> k = /* Cryptographically random nonce */;
ap_uint<256> r, s;

bool signSuccess = xf::security::nistp256Sign(messageHash, k, privateKey, r, s);

// Verify signature
ap_uint<256> publicKeyX = /* Public key X coordinate */;
ap_uint<256> publicKeyY = /* Public key Y coordinate */;

bool verifySuccess = xf::security::nistp256Verify(r, s, messageHash, 
                                                   publicKeyX, publicKeyY);
```

## Additional Resources

For more information about the Vitis Security Library and FPGA-based cryptography:
- [Vitis Libraries Documentation](https://xilinx.github.io/Vitis_Libraries/)
- [FPGA Cryptography Best Practices](https://www.xilinx.com/applications/security.html)
