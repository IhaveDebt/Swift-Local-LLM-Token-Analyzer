import Foundation

// MARK: - Tokenizer
class SimpleTokenizer {
    func tokenize(_ text: String) -> [String] {
        var buffer = ""
        var tokens: [String] = []
        
        for char in text {
            if char.isWhitespace {
                if !buffer.isEmpty { tokens.append(buffer); buffer = "" }
            } else if char.isPunctuation {
                if !buffer.isEmpty { tokens.append(buffer); buffer = "" }
                tokens.append(String(char))
            } else {
                buffer.append(char)
            }
        }
        
        if !buffer.isEmpty { tokens.append(buffer) }
        return tokens
    }
}

// MARK: - Token Statistics
struct TokenStats {
    let total: Int
    let unique: Int
    let frequencies: [String: Int]
}

class TokenAnalyzer {
    private let tokenizer = SimpleTokenizer()
    
    func analyze(text: String) -> TokenStats {
        let tokens = tokenizer.tokenize(text)
        var freq: [String: Int] = [:]
        
        tokens.forEach { freq[$0, default: 0] += 1 }
        
        return TokenStats(
            total: tokens.count,
            unique: freq.count,
            frequencies: freq
        )
    }
    
    func prettyPrint(_ stats: TokenStats) {
        print("TOTAL TOKENS:", stats.total)
        print("UNIQUE TOKENS:", stats.unique)
        print("TOP 10:")
        stats.frequencies.sorted { $0.value > $1.value }
            .prefix(10)
            .forEach { print("\($0.key): \($0.value)") }
    }
}

// MARK: - Demo
let text = """
Large Language Models work using tokens, not words.
This analyzer shows how many tokens your text contains.
"""

let analyzer = TokenAnalyzer()
let stats = analyzer.analyze(text: text)
analyzer.prettyPrint(stats)
