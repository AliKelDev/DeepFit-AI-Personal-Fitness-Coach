# DeepFit - AI Personal Fitness Coach

<img width="1206" alt="Screenshot 2025-02-25 at 19 09 59" src="https://github.com/user-attachments/assets/da6398d7-cd64-4479-9767-b9c22726cfec" />

## Overview

DeepFit is an intelligent AI personal trainer that provides personalized workout plans, real-time form feedback, and progress tracking. Developed by [Alikearn Studio](https://webpixelle3.netlify.app/portfolio), DeepFit uses advanced AI models to create a personalized fitness experience tailored to your goals, equipment, and fitness level.

**[Live Demo](https://deepfit-alikearn.netlify.app/)**

## Features

### 🏋️ Personal AI Coach - Max

Meet Max, your AI fitness coach who offers:
- Personalized workout recommendations based on your profile
- Exercise form guidance and corrections
- Answers to any fitness-related questions
- Training advice adapted to your specific situation

### 📊 Progress Analytics

Track your fitness journey with detailed analytics:
- Strength progression graphs for each exercise
- Volume tracking across workouts
- Muscle group balance visualization
- Personal record tracking
- Weekly workout frequency analysis

### 📝 Custom Workout Creation

Build the perfect workout routine:
- Comprehensive exercise database organized by muscle groups
- Set tracking with different types (normal, warm-up, drop sets)
- Weight and rep progression tracking
- Save and reuse your favorite routines

### 📱 Athlete Profiles

Create detailed profiles that help Max understand your needs:
- Fitness level assessment
- Equipment availability tracking
- Physical limitation accommodation
- Training history integration

## Technology Stack

- **Frontend**: React, Framer Motion
- **State Management**: React Context API
- **Styling**: TailwindCSS
- **AI Integration**: Gemini API, Moondream API
- **Deployment**: Netlify Functions, Netlify Hosting

## How It Works

### AI Coach Integration

DeepFit uses a combination of AI models to deliver personalized coaching. The application sends user context, questions, and optionally image analysis to provide contextually relevant responses.

```javascript
// Simplified AI chat integration
const aiMessages = [
  {
    role: "system",
    content: `You are Max, a certified personal trainer and sports coach. 
    You're knowledgeable about fitness and sports, but you're also a 
    well-rounded conversation partner with diverse interests and knowledge.
    
    USER PROFILE:
    - Name: ${userProfile.name}
    - Age: ${userProfile.age}
    - Fitness Level: ${fitnessLevel}
    - Physical Limitations: ${physicalLimitations.join(', ')}
    - Available Equipment: ${equipment.join(', ')}
    `
  }
];

// Add user message with context
if (imageAnalysis) {
  userPrompt = `[Image Analysis: ${imageAnalysis}]\n\n${userPrompt}`;
}

// Send to AI API and get response
const response = await fetch('/.netlify/functions/ai-chat', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    messages: aiMessages,
    imageAnalysis,
    userProfile
  })
});
```

### Progress Tracking System

The app tracks workout performance over time to visualize progress and provide insights for improvement.

```javascript
// Sample progress visualization logic
const getExerciseProgressData = () => {
  if (!selectedExercise) return [];
  
  const filteredHistory = getFilteredHistory();
  const progressData = [];
  
  filteredHistory.forEach(workout => {
    workout.exercises.forEach(exercise => {
      if (exercise.name === selectedExercise) {
        // Find the heaviest completed set
        let maxWeight = 0;
        let volume = 0;
        
        exercise.sets.forEach(set => {
          if (set.completed) {
            const weight = parseFloat(set.actualWeight);
            const reps = parseInt(set.actualReps);
            
            if (!isNaN(weight) && !isNaN(reps)) {
              if (weight > maxWeight) maxWeight = weight;
              volume += weight * reps;
            }
          }
        });
        
        if (maxWeight > 0) {
          progressData.push({
            date: new Date(workout.startTime).toLocaleDateString(),
            timestamp: new Date(workout.startTime).getTime(),
            weight: maxWeight,
            volume: volume
          });
        }
      }
    });
  });
  
  // Sort by date and return for visualization
  return progressData.sort((a, b) => a.timestamp - b.timestamp);
};
```

### Workout Tracking Architecture

DeepFit uses a context-based system to manage workout data across components:

```jsx
// WorkoutContext provider excerpt
export const WorkoutProvider = ({ children }) => {
  const [workouts, setWorkouts] = useState([]);
  const [workoutHistory, setWorkoutHistory] = useState([]);
  const [activeWorkout, setActiveWorkout] = useState(null);
  const [userProfile, setUserProfile] = useState(null);

  // Complete a workout session
  const completeWorkout = () => {
    if (!activeWorkout) return;
    
    const completedWorkout = {
      ...activeWorkout,
      endTime: new Date().toISOString(),
      duration: (new Date() - new Date(activeWorkout.startTime)) / 1000,
      isCompleted: true
    };
    
    setWorkoutHistory(prev => [completedWorkout, ...prev]);
    setActiveWorkout(null);
    
    return completedWorkout;
  };

  // Other workout management functions...

  return (
    <WorkoutContext.Provider
      value={{
        workouts,
        workoutHistory,
        activeWorkout,
        userProfile,
        createWorkout,
        updateWorkout,
        deleteWorkout,
        startWorkout,
        completeWorkout,
        updateWorkoutSet
      }}
    >
      {children}
    </WorkoutContext.Provider>
  );
};
```

## Screenshots

<img width="924" alt="Screenshot 2025-02-25 at 19 18 15" src="https://github.com/user-attachments/assets/ca589b21-b62d-4d96-8616-30b279b2ad11" />

<img width="923" alt="Screenshot 2025-02-25 at 19 18 33" src="https://github.com/user-attachments/assets/a6255d78-f8d6-4025-8e5b-ff0bc8981e60" />

<img width="950" alt="Screenshot 2025-02-25 at 19 18 45" src="https://github.com/user-attachments/assets/f88c25f7-a82b-414c-aa12-459d842dd8ee" />

<img width="1181" alt="Screenshot 2025-02-25 at 19 23 00" src="https://github.com/user-attachments/assets/acae8352-8ec9-432e-88c4-2ac347b3b03e" />

<img width="1128" alt="Screenshot 2025-02-25 at 19 23 13" src="https://github.com/user-attachments/assets/fda414a8-3941-4123-a3cb-c2964b4cff45" />

<img width="1136" alt="Screenshot 2025-02-25 at 19 23 46" src="https://github.com/user-attachments/assets/f573c34e-cb41-4a61-968a-ac4afed3fa59" />

<img width="1180" alt="Screenshot 2025-02-25 at 19 23 56" src="https://github.com/user-attachments/assets/6646a8f2-38fc-471f-9d44-1bc2b561a7b1" />



## Future Development

- Integration with fitness wearables for automatic data collection
- Video analysis for real-time form feedback
- Social features to train with friends
- Expanded exercise database with video demonstrations
- Nutrition tracking and meal planning

## About the Author

**Jordan Montée (Alikel/AlikelDev)** is a developer focused on creating AI-powered applications that provide real value to users. As the founder of Alikearn Studio, I develop AI assistants and applications that enhance everyday experiences.

## Related Projects

- [DeepChef](https://deep-chef.netlify.app/) - AI culinary assistant ([GitHub](https://github.com/AliKelDev/DeepChef-Auguste-Cooking-assistant))
- [Alikearn Studio Portfolio](https://webpixelle3.netlify.app/portfolio)

## License

This project is presented for demonstration purposes. The actual codebase is proprietary and not open-source. The concepts, designs, and code snippets shared in this repository are © 2025 Alikearn Studio.

---

<p align="center">
  <a href="https://deepfit-alikearn.netlify.app/">Live Demo</a> •
  <a href="https://webpixelle3.netlify.app/portfolio">Alikearn Studio</a> •
  <a href="https://github.com/AliKelDev">GitHub</a>
</p>
