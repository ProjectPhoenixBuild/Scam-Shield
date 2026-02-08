# scam_detector.py
import re

class ScamDetector:
    def __init__(self):
        self.scam_patterns = [
            (r'(won|winner|prize|lottery).*?(click|link|call)', 0.8),
            (r'(urgent|immediately|right now).*?(send|payment|money)', 0.7),
            (r'(bank account|upi|pin|password).*?(share|tell|provide)', 0.9),
            (r'(government|pm|modi).*?(scheme|benefit|free)', 0.6),
            (r'(investment|double.*money|guaranteed.*return)', 0.85)
        ]
    
    def analyze(self, text):
        score = 0
        reasons = []
        
        for pattern, weight in self.scam_patterns:
            if re.search(pattern, text, re.IGNORECASE):
                score += weight
                reasons.append(f"Matched pattern: {pattern}")
        
        confidence = min(score / len(self.scam_patterns), 1.0)
        
        if confidence > 0.7:
            verdict = "🚨 HIGH RISK: Likely scam"
        elif confidence > 0.4:
            verdict = "⚠️ MEDIUM RISK: Be cautious"
        else:
            verdict = "✅ LOW RISK: Seems legitimate"
        
        return {
            "confidence": round(confidence * 100, 2),
            "verdict": verdict,
            "reasons": reasons[:3]  # Top 3 reasons
        }

# Quick test
if __name__ == "__main__":
    detector = ScamDetector()
    
    test_messages = [
        "Congratulations! You won 50 lakhs! Click here to claim.",
        "Your bank account needs verification. Share your PIN immediately.",
        "Hello, just checking how you're doing."
    ]
    
    for msg in test_messages:
        result = detector.analyze(msg)
        print(f"Message: {msg[:50]}...")
        print(f"Result: {result['verdict']} ({result['confidence']}%)")
        print("---")
