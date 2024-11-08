# Skin Health Analyzer Using Llama 3.2

## Project Overview
A skin condition analysis tool that uses Llama 3.2's multimodal capabilities to:
- Process skin condition photos
- Understand user symptom descriptions
- Provide initial analysis while maintaining medical safety

## Technical Implementation

### 1. Input Processing
```python
class InputProcessor:
    def __init__(self):
        self.model = load_llama_model()  # Provided by hackathon
        
    def process_inputs(self, image=None, text=None):
        inputs = {
            "image_input": self.process_image(image) if image else None,
            "text_input": text if text else None
        }
        return inputs

    def process_image(self, image):
        # Convert to format expected by Llama 3.2
        return preprocess_image(image)  # Use Llama's image preprocessing

class SkinAnalyzer:
    def __init__(self):
        self.processor = InputProcessor()
        self.safety_checker = SafetyChecker()
    
    async def analyze(self, image=None, description=None):
        # Process inputs
        inputs = self.processor.process_inputs(image, description)
        
        # Generate prompt for Llama 3.2
        prompt = self.create_medical_prompt(inputs)
        
        # Get analysis from model
        analysis = await self.model.generate(prompt)
        
        # Safety check
        safe_response = self.safety_checker.validate(analysis)
        
        return safe_response

    def create_medical_prompt(self, inputs):
        """Create specialized medical prompt for Llama 3.2"""
        prompt = """Analyze this skin condition with medical context. 
        Consider visual symptoms from the image and described symptoms.
        Provide analysis with appropriate medical disclaimers."""
        
        return prompt
```

### 2. Safety Layer
```python
class SafetyChecker:
    def __init__(self):
        self.emergency_terms = [
            "spreading rapidly", "severe pain",
            "difficulty breathing", "severe swelling"
        ]
    
    def validate(self, analysis):
        response = {
            "analysis": analysis,
            "disclaimer": "This is not medical advice. Please consult a healthcare professional.",
            "seek_professional": False,
            "emergency": False
        }
        
        # Check for emergency conditions
        if any(term in analysis.lower() for term in self.emergency_terms):
            response["seek_professional"] = True
            response["emergency"] = True
            
        return response
```

## Implementation Timeline

### Day 1: Core Features
1. Set up Llama 3.2 integration
2. Implement basic image/text input processing
3. Create medical analysis prompts
4. Basic safety checks

### Day 2: Enhancement & Testing
1. Improve prompt engineering
2. Add emergency detection
3. Enhance response formatting
4. Testing with various conditions

## Safety Features
- Medical disclaimers
- Emergency condition detection
- Professional consultation recommendations
- Data privacy through on-device processing

## Out of Scope for MVP
❌ Custom model training
❌ Complex medical recommendations
❌ Condition treatment advice
❌ Historical analysis
