### The challenge:


```
ZGlnICtzaG9ydCBlZ2c/Pz8/LnBocmFjay5vcmcgVFhU
```


---

### THE SOLUTION:

```rust
use std::{process::Command, str::from_utf8};

fn main() -> Result<(), String> {
    let encoded = "ZGlnICtzaG9ydCBlZ2c/Pz8/LnBocmFjay5vcmcgVFhU";

    let decoded_bytes = decode_base64(encoded.as_bytes())?;
    let decoded_str = from_utf8(&decoded_bytes).map_err(|e| e.to_string())?;

    let command = decoded_str.replace("????", "1337");

    println!("Executing: {}", command);

    let parts: Vec<_> = command.split_whitespace().collect();
    if parts.is_empty() {
        return Err("Empty command".into());
    }

    let output = Command::new(parts[0])
                        .args(&parts[1..])
                        .output()
                        .map_err(|e| e.to_string())?;

    println!("Output: {}", String::from_utf8_lossy(&output.stdout));
    Ok(())
}

fn decode_base64(input: &[u8]) -> Result<Vec<u8>, String> {
    let table = |c: u8| -> Result<u8, String> {
        match c {
            b'A'..=b'Z' => Ok(c - b'A'),
            b'a'..=b'z' => Ok(c - b'a' + 26),
            b'0'..=b'9' => Ok(c - b'0' + 52),
            b'+' => Ok(62),
            b'/' => Ok(63),
            b'=' => Ok(0),
            _ => Err(format!("Invalid base64 character: {}", c)),
        }
    };

    let padding = input.iter().rev().take_while(|&&c| c == b'=').count();
    let output_size = input.len() / 4 * 3 - padding;
    let mut result = Vec::with_capacity(output_size);
    let mut chunks = input.chunks_exact(4);

    for chunk in &mut chunks {
        let n1 = table(chunk[0])?;
        let n2 = table(chunk[1])?;

        result.push((n1 << 2) | (n2 >> 4));

        if chunk[2] != b'=' {
            let n3 = table(chunk[2])?;
            result.push((n2 << 4) | (n3 >> 2));

            if chunk[3] != b'=' {
                let n4 = table(chunk[3])?;
                result.push((n3 << 6) | n4);
            }
        }
    }

    let remainder = chunks.remainder();
    if !remainder.is_empty() {
        return Err("Invalid base64 input length".into());
    }

    Ok(result)
}
```
