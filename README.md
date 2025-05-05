# AI-Projects
Projects made with AI
"use client"

import { useState } from "react"

type AnalysisResult = {
  summary: string
  trends: string[]
  sentiment: "positive" | "negative" | "neutral"
  recommendations: string[]
}

export function useGrokAnalysis() {
  const [isLoading, setIsLoading] = useState(false)
  const [error, setError] = useState<string | null>(null)
  const [result, setResult] = useState<AnalysisResult | null>(null)

  const analyzeWithGrok = async (query: string, newsData: any[]) => {
    setIsLoading(true)
    setError(null)

    try {
      const response = await fetch("/api/grok-analysis", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ query, newsData }),
      })

      if (!response.ok) {
        throw new Error(`Error: ${response.status}`)
      }

      const data = await response.json()
      setResult(data)
      return data
    } catch (err) {
      setError(err instanceof Error ? err.message : "An unknown error occurred")
      return null
    } finally {
      setIsLoading(false)
    }
  }

  return {
    analyzeWithGrok,
    isLoading,
    error,
    result,
  }
}
