# Code Quality Review Prompt

## Description
This prompt helps perform comprehensive code quality reviews across various programming languages.

## Use Case
Code reviews, quality assessments, and improvement recommendations for any codebase.

## Author
AWS Q Developer SA Team

## Version
1.0.0 (2025-04-24)

## Instructions

When I provide you with code for review, please:

1. Perform a comprehensive code quality assessment focusing on:
   - Code structure and organization
   - Readability and maintainability
   - Performance considerations
   - Memory safety and ownership patterns
   - Error handling with Result/Option types
   - Adherence to Rust idioms and best practices
   - Cargo.toml dependencies and feature usage
   - Documentation and testing coverage

2. Identify specific issues categorized by severity:
   - Critical: Security vulnerabilities, severe performance issues, or potential crashes
   - Major: Code smells, maintainability issues, or inefficient implementations
   - Minor: Style inconsistencies, documentation gaps, or minor optimizations

3. For each issue identified:
   - Specify the exact location (file, line number)
   - Explain why it's problematic
   - Provide a recommended solution with code examples
   - Reference relevant best practices or standards

4. Suggest improvements beyond fixing issues:
   - Zero-cost abstraction opportunities
   - Refactoring suggestions for better ownership patterns
   - Modern Rust features that could be utilized (async/await, const generics, etc.)
   - Performance optimization techniques (avoiding allocations, using iterators)
   - Additional tests and benchmarks that should be added
   - Clippy lint suggestions and cargo fmt compliance

5. Highlight positive aspects of the code:
   - Well-implemented patterns or techniques
   - Particularly elegant or efficient solutions
   - Good documentation or commenting practices
   - Effective error handling or validation

6. Provide a summary that includes:
   - Overall code quality assessment
   - Key strengths and weaknesses
   - Prioritized list of recommended changes
   - Estimated effort for implementing changes

## Example Input

```rust
use std::collections::HashMap;

#[derive(Debug)]
struct Item {
    price: f64,
    quantity: u32,
}

#[derive(Debug)]
struct User {
    user_type: String,
}

fn calculate_total(items: Vec<Item>) -> f64 {
    let mut total = 0.0;
    for i in 0..items.len() {
        total = total + items[i].price * items[i].quantity as f64;
    }
    total
}

fn apply_discount(total: f64, user: User) -> f64 {
    if user.user_type == "premium" {
        return total * 0.9;
    }
    if user.user_type == "gold" {
        return total * 0.8;
    }
    total
}

fn process_order(items: Vec<Item>, user: User) -> f64 {
    let total = calculate_total(items);
    let final_total = apply_discount(total, user);
    println!("Order processed. Total: ${}", final_total);
    final_total
}
```

## Example Output

**Code Quality Review Summary**

**Overall Assessment:**
The code is functional but has several opportunities for improvement in terms of Rust idioms, performance, and error handling. The structure is simple but doesn't leverage Rust's ownership system and iterator patterns effectively.

**Strengths:**
- Clear function names that describe their purpose
- Simple, focused functions with single responsibilities
- Proper use of structs for data modeling
- Memory-safe by default due to Rust's ownership system

**Issues by Severity:**

**Major Issues:**

1. **Inefficient Ownership Patterns**
   - Location: `calculate_total()` and `apply_discount()` functions
   - Problem: Taking ownership of Vec<Item> and User when borrowing would suffice
   - Recommendation: Use references to avoid unnecessary moves

   ```rust
   fn calculate_total(items: &[Item]) -> f64 {
       items.iter()
           .map(|item| item.price * item.quantity as f64)
           .sum()
   }
   
   fn apply_discount(total: f64, user: &User) -> f64 {
       // Implementation
   }
   ```

2. **Non-idiomatic Iteration**
   - Location: `calculate_total()` function
   - Problem: Using index-based iteration instead of iterators
   - Recommendation: Use iterator methods for better performance and readability

**Minor Issues:**

1. **String Comparison for User Type**
   - Location: `apply_discount()` function
   - Problem: Using String comparison instead of enum
   - Recommendation: Use enum for better type safety

   ```rust
   #[derive(Debug, Clone, Copy)]
   enum UserType {
       Premium,
       Gold,
       Standard,
   }
   
   impl UserType {
       fn discount_rate(&self) -> f64 {
           match self {
               UserType::Premium => 0.9,
               UserType::Gold => 0.8,
               UserType::Standard => 1.0,
           }
       }
   }
   ```

2. **Missing Error Handling**
   - Location: All functions
   - Problem: No error handling for edge cases (empty items, invalid prices)
   - Recommendation: Use Result<T, E> for fallible operations

3. **Floating Point for Currency**
   - Location: Item struct
   - Problem: Using f64 for monetary values can lead to precision issues
   - Recommendation: Consider using a decimal crate or integer cents

**Improvement Suggestions:**

1. **Leverage Iterator Patterns**
   ```rust
   fn calculate_total(items: &[Item]) -> f64 {
       items.iter()
           .map(|item| item.price * f64::from(item.quantity))
           .sum()
   }
   ```

2. **Add Documentation**
   ```rust
   /// Calculates the total price for a collection of items.
   /// 
   /// # Arguments
   /// * `items` - A slice of items to calculate the total for
   /// 
   /// # Returns
   /// The total price as a floating-point number
   fn calculate_total(items: &[Item]) -> f64 {
       // Implementation
   }
   ```

3. **Add Comprehensive Tests**
   ```rust
   #[cfg(test)]
   mod tests {
       use super::*;
   
       #[test]
       fn test_calculate_total_empty() {
           assert_eq!(calculate_total(&[]), 0.0);
       }
   
       #[test]
       fn test_apply_discount_premium() {
           let user = User { user_type: UserType::Premium };
           assert_eq!(apply_discount(100.0, &user), 90.0);
       }
   }
   ```

4. **Consider Using Clippy**
   - Run `cargo clippy` to catch additional Rust-specific issues
   - Enable additional lints in Cargo.toml for stricter checking

**Prioritized Recommendations:**
1. Refactor to use borrowing instead of ownership
2. Replace index-based iteration with iterator methods
3. Use enums instead of string comparisons
4. Add proper error handling with Result types
5. Add documentation and comprehensive tests
6. Consider using decimal types for currency

**Estimated Effort:**
Medium - These changes could be implemented in 2-3 hours by a developer familiar with Rust idioms.
