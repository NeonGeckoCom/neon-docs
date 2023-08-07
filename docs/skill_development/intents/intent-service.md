# Intent Service

Neon supports both Padatious and Adapt intent handlers. Neon Core has an intent service that decides which intent will be triggered by a particular utterance. This is based on the confidence of a spoken command or question, converted to text and normalized, and translated (in certain cases).

## Intent Confidence Order of Priority

1. Active skills attempt to handle using `converse()`
2. Padatious high match intents
3. Adapt intent handlers
4. Fallbacks:
   - Padatious near match intents
   - General fallbacks
   - Padatious loose match intents
   - Unknown intent handler
