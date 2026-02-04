# Requirements Document

## Introduction

InkFlow AI is a structured 1-on-1 AI tutoring platform that behaves like a real professor conducting a live class. The system follows a "Google Meet for AI Tutoring" design philosophy with minimalist UI, content-first approach, and clear iconography. The platform prioritizes teaching flow over raw information output through a curriculum-driven "Syllabus-First" teaching engine.

## Glossary

- **Teaching_Block**: A structured data unit containing lesson content, audio, visual elements, and UI state information
- **Syllabus_Engine**: The core system component that generates and manages structured learning paths
- **Whiteboard**: Interactive Fabric.js canvas for handwritten SVG stroke animations
- **Code_Editor**: Monaco-based editor for code demonstrations and user practice
- **VAD**: Voice Activity Detection system for handling user interruptions
- **Lesson_Controller**: State machine managing the teaching flow progression
- **UI_State**: Configuration object controlling interface behavior and user interaction permissions

## Requirements

### Requirement 1: Syllabus-First Teaching Engine

**User Story:** As a student, I want the AI to provide structured learning paths for broad topics, so that I can follow a logical progression instead of receiving overwhelming information dumps.

#### Acceptance Criteria

1. WHEN a user asks a broad topic question, THE Syllabus_Engine SHALL generate a structured syllabus outlining the complete learning path
2. WHEN a syllabus is generated, THE System SHALL present it as the first response before any detailed explanations
3. WHEN a syllabus item is completed, THE System SHALL automatically advance to the next item in sequence
4. THE Syllabus_Engine SHALL organize topics in logical dependency order (prerequisites before advanced concepts)
5. WHEN a user asks a narrow single-topic question, THE System SHALL provide direct explanation without syllabus generation

### Requirement 2: Sequential Topic Teaching Flow

**User Story:** As a student, I want each topic taught through a consistent Theory-Demo-Practice-Validation cycle, so that I can learn effectively through multiple reinforcement methods.

#### Acceptance Criteria

1. WHEN teaching a syllabus topic, THE System SHALL execute the sequence: Theory → Demo → Practice → Validation
2. WHEN in Theory phase, THE System SHALL activate the Whiteboard and explain concepts using synchronized voice and handwritten SVG strokes
3. WHEN in Demo phase, THE System SHALL switch to Code Editor mode and type example code character-by-character synchronized with audio narration
4. WHEN in Practice phase, THE System SHALL unlock the editor for user input and provide a clear task prompt
5. WHEN in Validation phase, THE System SHALL evaluate user code and provide feedback before advancing

### Requirement 3: Interactive Whiteboard System

**User Story:** As a student, I want visual explanations with handwritten animations, so that I can better understand concepts through synchronized audio-visual learning.

#### Acceptance Criteria

1. THE Whiteboard SHALL use Fabric.js canvas with deep indigo background
2. WHEN explaining concepts, THE System SHALL render handwritten SVG stroke animations
3. WHEN rendering handwriting, THE System SHALL synchronize SVG path animation with voice narration timing
4. THE Whiteboard SHALL support laser-pointer trace effects following the SVG animation path
5. WHEN transitioning to code mode, THE Whiteboard SHALL slide out smoothly using Framer Motion animations

### Requirement 4: Code Editor Integration

**User Story:** As a student, I want to see code demonstrations and practice coding myself, so that I can learn through hands-on experience.

#### Acceptance Criteria

1. THE Code_Editor SHALL use Monaco editor with VS Code Dark Theme
2. WHEN demonstrating code, THE System SHALL implement typewriter effect synchronized to audio duration
3. WHEN in practice mode, THE System SHALL unlock the editor for user input
4. WHEN user clicks Run, THE System SHALL execute code validation against expected patterns
5. THE Code_Editor SHALL support pattern-based validation rather than strict string matching

### Requirement 5: Teaching Block Data Structure

**User Story:** As a developer, I want structured lesson content delivery, so that the UI can render consistent teaching experiences.

#### Acceptance Criteria

1. THE System SHALL stream all lesson content as structured Teaching_Block objects
2. WHEN generating content, THE System SHALL include block_id, type, content, and ui_state fields
3. THE System SHALL support block types: SYLLABUS, THEORY, DEMO, and TASK
4. WHEN sending syllabus blocks, THE System SHALL include syllabus_list with ordered topic names
5. WHEN sending content blocks, THE System SHALL include audio_text, handwriting_svg, and code_snippet fields

### Requirement 6: Voice Activity Detection and Interruption Handling

**User Story:** As a student, I want to interrupt the AI with questions, so that I can get clarification without losing my place in the lesson.

#### Acceptance Criteria

1. THE VAD SHALL implement client-side voice activity detection
2. WHEN user speech is detected, THE System SHALL pause audio playback immediately
3. WHEN user interrupts, THE System SHALL pause typing and handwriting animations
4. WHEN user interrupts, THE System SHALL clear pending Teaching_Block queue
5. WHEN handling interruptions, THE System SHALL provide context-aware responses and offer lesson resumption

### Requirement 7: Automatic Progression and Silence Handling

**User Story:** As a student, I want the lesson to continue automatically when I understand a topic, so that I don't need to manually advance through every step.

#### Acceptance Criteria

1. WHEN a topic completes and user remains silent, THE System SHALL wait 5 seconds before auto-advancing
2. WHEN validation passes, THE System SHALL provide brief praise and automatically load the next syllabus topic
3. WHEN validation fails, THE System SHALL explain the mistake on the Whiteboard and prompt retry
4. THE System SHALL track completion status for each syllabus item
5. WHEN all syllabus items complete, THE System SHALL provide lesson summary and next steps

### Requirement 8: Meet-Like User Interface

**User Story:** As a student, I want a clean, professional interface similar to video conferencing tools, so that I can focus on learning without UI distractions.

#### Acceptance Criteria

1. THE UI SHALL allocate 90% of screen space to the main stage with split container layout
2. THE UI SHALL display Whiteboard on the left and Code_Editor on the right in split mode
3. THE UI SHALL provide a floating glass-style control bar in the bottom 10% of screen
4. THE Control_Bar SHALL include icons for: Mic Toggle, Camera Toggle, Hand Raise, and End Session
5. THE UI SHALL use Lucide React icons exclusively with no emoji usage

### Requirement 9: Session State Management

**User Story:** As a student, I want my learning progress tracked within a session, so that interruptions don't lose my place in the curriculum.

#### Acceptance Criteria

1. THE System SHALL store generated syllabus in session_plan using DynamoDB
2. WHEN a session starts, THE System SHALL track current topic position and completion status
3. WHEN user interrupts, THE System SHALL maintain session context for resumption
4. THE System SHALL persist user code attempts and validation results within the session
5. WHEN session ends, THE System SHALL provide progress summary and continuation options

### Requirement 10: Code Validation System

**User Story:** As a student, I want my practice code evaluated intelligently, so that I receive helpful feedback rather than rigid exact-match requirements.

#### Acceptance Criteria

1. THE Validation_System SHALL implement pattern-based code evaluation
2. WHEN validating user code, THE System SHALL compare against expected patterns rather than exact strings
3. WHEN code is correct, THE System SHALL provide brief positive feedback and advance automatically
4. WHEN code is incorrect, THE System SHALL identify specific mistakes and provide targeted guidance
5. THE Validation_System SHALL support multiple correct solution approaches for the same problem

### Requirement 11: Audio-Visual Synchronization

**User Story:** As a student, I want perfectly synchronized audio and visual elements, so that the learning experience feels natural and professional.

#### Acceptance Criteria

1. WHEN playing handwriting animations, THE System SHALL synchronize SVG path rendering with audio narration timing
2. WHEN demonstrating code, THE System SHALL match typewriter effect speed to audio duration
3. THE System SHALL support pause and resume functionality that maintains audio-visual synchronization
4. WHEN user interrupts, THE System SHALL resume from the exact synchronization point after handling the interruption
5. THE System SHALL buffer audio and visual content to prevent desynchronization due to network latency

### Requirement 12: Curriculum Router Backend

**User Story:** As a system, I want intelligent lesson plan generation, so that broad topics are structured appropriately while narrow topics receive direct responses.

#### Acceptance Criteria

1. THE Curriculum_Router SHALL detect whether user requests are broad topics or narrow questions
2. WHEN processing broad topics, THE System SHALL generate comprehensive syllabus using generateLessonPlan function
3. WHEN processing narrow questions, THE System SHALL provide direct single-topic explanations
4. THE System SHALL store lesson plans in DynamoDB with session association
5. WHEN lesson plan is generated, THE System SHALL immediately emit the first topic Teaching_Block