import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";

export default function ExamApp() {
  const [numQuestions, setNumQuestions] = useState(5);
  const [penaltyWrong, setPenaltyWrong] = useState(1);
  const [penaltyUnanswered, setPenaltyUnanswered] = useState(0.5);
  const [answers, setAnswers] = useState(Array(numQuestions).fill(""));
  const [score, setScore] = useState(null);

  const correctAnswers = ["A", "B", "C", "D", "A"]; // Simulación de respuestas correctas

  const handleGrade = () => {
    let totalScore = 0;
    answers.forEach((answer, index) => {
      if (answer === correctAnswers[index]) totalScore += 1;
      else if (answer === "") totalScore -= penaltyUnanswered;
      else totalScore -= penaltyWrong;
    });
    setScore(totalScore);
  };

  return (
    <div className="p-6 max-w-xl mx-auto">
      <Card>
        <CardContent className="p-4">
          <h2 className="text-xl font-bold mb-4">Cartilla de Examen</h2>
          {answers.map((_, index) => (
            <div key={index} className="mb-2">
              <Label>Pregunta {index + 1}</Label>
              <Input
                type="text"
                placeholder="Ingresa tu respuesta (A, B, C, D)"
                value={answers[index]}
                onChange={(e) => {
                  const newAnswers = [...answers];
                  newAnswers[index] = e.target.value.toUpperCase();
                  setAnswers(newAnswers);
                }}
              />
            </div>
          ))}
          <Button onClick={handleGrade} className="mt-4">Calificar</Button>
          {score !== null && <p className="mt-4 text-lg">Puntaje final: {score}</p>}
        </CardContent>
      </Card>
    </div>
  );
}
