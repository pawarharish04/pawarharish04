import { useEffect, useRef, useState } from "react";

const techs = [
  { name: "C++", color: "#00599C", bg: "#001f3f", icon: "⚙️" },
  { name: "JavaScript", color: "#F7DF1E", bg: "#1a1400", icon: "⚡" },
  { name: "React", color: "#61DAFB", bg: "#001a20", icon: "⚛️" },
  { name: "Node.js", color: "#6DA55F", bg: "#0d1a0a", icon: "🟢" },
  { name: "Express", color: "#c9d1d9", bg: "#1a1a1a", icon: "🚀" },
  { name: "PostgreSQL", color: "#336791", bg: "#0a1020", icon: "🐘" },
  { name: "MongoDB", color: "#47A248", bg: "#0a1a0a", icon: "🍃" },
  { name: "MySQL", color: "#4479A1", bg: "#0a1020", icon: "🗄️" },
  { name: "TailwindCSS", color: "#38B2AC", bg: "#001a1a", icon: "🌊" },
  { name: "JWT", color: "#D63AFF", bg: "#1a0020", icon: "🔐" },
  { name: "Git", color: "#F05033", bg: "#1a0800", icon: "🌿" },
  { name: "VS Code", color: "#0078d7", bg: "#00101a", icon: "💙" },
];

function TechCard({ tech, angle, radius }) {
  const rad = (angle * Math.PI) / 180;
  const x = Math.cos(rad) * radius;
  const y = Math.sin(rad) * radius;
  const [hovered, setHovered] = useState(false);

  return (
    <div
      style={{
        position: "absolute",
        left: "50%",
        top: "50%",
        transform: `translate(calc(-50% + ${x}px), calc(-50% + ${y}px))`,
        zIndex: 10,
      }}
    >
      <div
        onMouseEnter={() => setHovered(true)}
        onMouseLeave={() => setHovered(false)}
        style={{
          background: tech.bg,
          border: `1.5px solid ${hovered ? tech.color + "99" : tech.color + "44"}`,
          borderRadius: "50%",
          width: 68,
          height: 68,
          display: "flex",
          flexDirection: "column",
          alignItems: "center",
          justifyContent: "center",
          boxShadow: hovered
            ? `0 0 28px ${tech.color}99, inset 0 0 16px ${tech.color}22`
            : `0 0 12px ${tech.color}33`,
          cursor: "pointer",
          transform: hovered ? "scale(1.2)" : "scale(1)",
          transition: "all 0.3s ease",
        }}
      >
        <span style={{ fontSize: 20, lineHeight: 1 }}>{tech.icon}</span>
        <span style={{
          color: tech.color,
          fontSize: 8,
          fontWeight: 700,
          fontFamily: "'JetBrains Mono', 'Courier New', monospace",
          marginTop: 3,
          letterSpacing: "0.05em",
          textAlign: "center",
          maxWidth: 58,
          lineHeight: 1.2,
        }}>
          {tech.name}
        </span>
      </div>
    </div>
  );
}

function TechOrbit() {
  const [angle, setAngle] = useState(0);
  const [paused, setPaused] = useState(false);
  const rafRef = useRef();
  const lastRef = useRef();
  const angleRef = useRef(0);
  const radius = 195;

  useEffect(() => {
    const step = (ts) => {
      if (!lastRef.current) lastRef.current = ts;
      const delta = ts - lastRef.current;
      lastRef.current = ts;
      if (!paused) {
        angleRef.current = (angleRef.current + delta * 0.016) % 360;
        setAngle(angleRef.current);
      }
      rafRef.current = requestAnimationFrame(step);
    };
    rafRef.current = requestAnimationFrame(step);
    return () => cancelAnimationFrame(rafRef.current);
  }, [paused]);

  const size = radius * 2 + 120;

  return (
    <div style={{ display: "flex", flexDirection: "column", alignItems: "center" }}>
      <div
        style={{ position: "relative", width: size, height: size, maxWidth: "100%" }}
        onMouseEnter={() => setPaused(true)}
        onMouseLeave={() => setPaused(false)}
      >
        <svg style={{ position: "absolute", inset: 0, width: "100%", height: "100%", pointerEvents: "none" }} viewBox={`0 0 ${size} ${size}`}>
          <defs>
            <radialGradient id="cg" cx="50%" cy="50%" r="50%">
              <stop offset="0%" stopColor="#58A6FF" stopOpacity="0.12" />
              <stop offset="100%" stopColor="#58A6FF" stopOpacity="0" />
            </radialGradient>
          </defs>
          <circle cx={size / 2} cy={size / 2} r={radius} fill="none" stroke="#58A6FF" strokeOpacity="0.1" strokeWidth="1" strokeDasharray="4 10" />
          <circle cx={size / 2} cy={size / 2} r={70} fill="url(#cg)" />
        </svg>

        <div style={{
          position: "absolute", top: "50%", left: "50%",
          transform: "translate(-50%, -50%)", zIndex: 20, textAlign: "center",
        }}>
          <div style={{
            width: 96, height: 96, borderRadius: "50%",
            background: "linear-gradient(135deg, #1a1a2e, #16213e)",
            border: "1.5px solid #58A6FF44",
            display: "flex", flexDirection: "column",
            alignItems: "center", justifyContent: "center",
            boxShadow: "0 0 40px #58A6FF22",
          }}>
            <span style={{ fontSize: 28 }}>💻</span>
            <span style={{ color: "#58A6FF", fontSize: 8, fontWeight: 700, letterSpacing: "0.15em", marginTop: 4, fontFamily: "monospace" }}>HARISH</span>
          </div>
        </div>

        {techs.map((tech, i) => {
          const baseAngle = (360 / techs.length) * i;
          return (
            <TechCard key={tech.name} tech={tech} angle={(baseAngle + angle) % 360} radius={radius} />
          );
        })}
      </div>
      <p style={{ color: "#8b949e", fontSize: 10, letterSpacing: "0.2em", marginTop: 8, fontFamily: "monospace", opacity: 0.5 }}>
        ◈ HOVER TO PAUSE ◈
      </p>
    </div>
  );
}

function Section({ title, children }) {
  return (
    <div style={{ marginBottom: 56 }}>
      <div style={{ display: "flex", alignItems: "center", gap: 12, marginBottom: 24 }}>
        <div style={{ height: 1, flex: 1, background: "linear-gradient(to right, #58A6FF44, transparent)" }} />
        <h2 style={{ color: "#58A6FF", fontFamily: "monospace", fontSize: 13, letterSpacing: "0.25em", margin: 0, textTransform: "uppercase" }}>{title}</h2>
        <div style={{ height: 1, flex: 1, background: "linear-gradient(to left, #58A6FF44, transparent)" }} />
      </div>
      {children}
    </div>
  );
}

function ProjectCard({ emoji, title, subtitle, points, badges }) {
  const [hovered, setHovered] = useState(false);
  return (
    <div
      onMouseEnter={() => setHovered(true)}
      onMouseLeave={() => setHovered(false)}
      style={{
        background: hovered ? "#161b22" : "#0d1117",
        border: `1px solid ${hovered ? "#58A6FF44" : "#21262d"}`,
        borderRadius: 12,
        padding: "24px 28px",
        transition: "all 0.3s ease",
        boxShadow: hovered ? "0 0 30px #58A6FF11" : "none",
        flex: 1,
      }}
    >
      <div style={{ display: "flex", alignItems: "center", gap: 10, marginBottom: 6 }}>
        <span style={{ fontSize: 22 }}>{emoji}</span>
        <h3 style={{ color: "#e6edf3", margin: 0, fontSize: 16, fontFamily: "monospace", fontWeight: 700 }}>{title}</h3>
      </div>
      <p style={{ color: "#8b949e", fontSize: 12, fontStyle: "italic", marginBottom: 16, fontFamily: "monospace" }}>{subtitle}</p>
      <div style={{ background: "#010409", borderRadius: 8, padding: "14px 16px", marginBottom: 14, fontFamily: "monospace" }}>
        <div style={{ color: "#58A6FF", fontSize: 11, marginBottom: 8, letterSpacing: "0.15em" }}>Architecture Highlights</div>
        <div style={{ borderTop: "1px solid #21262d", paddingTop: 8 }}>
          {points.map((p, i) => (
            <div key={i} style={{ color: "#c9d1d9", fontSize: 12, lineHeight: 1.8 }}>
              <span style={{ color: "#58A6FF" }}>✦ </span>{p}
            </div>
          ))}
        </div>
      </div>
      <div style={{ display: "flex", gap: 8, flexWrap: "wrap" }}>
        {badges.map((b, i) => (
          <span key={i} style={{
            background: "#1c2128", border: "1px solid #30363d",
            color: "#8b949e", fontSize: 10, padding: "3px 10px",
            borderRadius: 20, fontFamily: "monospace", letterSpacing: "0.05em",
          }}>{b}</span>
        ))}
      </div>
    </div>
  );
}

function StatCard({ label, value, sub }) {
  return (
    <div style={{
      background: "#0d1117", border: "1px solid #21262d", borderRadius: 10,
      padding: "16px 20px", textAlign: "center", flex: 1,
    }}>
      <div style={{ color: "#58A6FF", fontSize: 22, fontWeight: 700, fontFamily: "monospace" }}>{value}</div>
      <div style={{ color: "#c9d1d9", fontSize: 11, marginTop: 4, fontFamily: "monospace" }}>{label}</div>
      {sub && <div style={{ color: "#8b949e", fontSize: 10, marginTop: 2, fontFamily: "monospace" }}>{sub}</div>}
    </div>
  );
}

export default function Portfolio() {
  return (
    <div style={{
      background: "#0d1117",
      minHeight: "100vh",
      color: "#c9d1d9",
      fontFamily: "monospace",
    }}>
      {/* Header */}
      <div style={{
        background: "linear-gradient(135deg, #0a0a0a 0%, #1a1a2e 50%, #16213e 100%)",
        padding: "64px 24px 56px",
        textAlign: "center",
        borderBottom: "1px solid #21262d",
        position: "relative",
        overflow: "hidden",
      }}>
        <div style={{
          position: "absolute", inset: 0, opacity: 0.04,
          backgroundImage: `repeating-linear-gradient(0deg, #58A6FF 0px, #58A6FF 1px, transparent 1px, transparent 40px),
                            repeating-linear-gradient(90deg, #58A6FF 0px, #58A6FF 1px, transparent 1px, transparent 40px)`,
        }} />
        <div style={{ position: "relative", zIndex: 1 }}>
          <div style={{
            display: "inline-block",
            background: "#58A6FF15",
            border: "1px solid #58A6FF33",
            borderRadius: 6,
            padding: "4px 14px",
            fontSize: 11,
            color: "#58A6FF",
            letterSpacing: "0.3em",
            marginBottom: 20,
          }}>
            ◈ FULL STACK DEVELOPER ◈ DSA ENTHUSIAST ◈ SYSTEMS ARCHITECT ◈
          </div>
          <h1 style={{
            fontSize: "clamp(36px, 6vw, 64px)",
            fontWeight: 800,
            color: "#ffffff",
            margin: "0 0 8px",
            letterSpacing: "0.02em",
            fontFamily: "monospace",
          }}>Harish Pawar</h1>
          <p style={{ color: "#8b949e", fontSize: 16, margin: "0 0 28px", letterSpacing: "0.1em" }}>
            Full Stack Engineer — Building What Matters
          </p>
          <div style={{ display: "flex", justifyContent: "center", gap: 12, flexWrap: "wrap" }}>
            {["JWT Auth", "RBAC", "Real-Time Architecture", "C++", "PostgreSQL", "React"].map(tag => (
              <span key={tag} style={{
                background: "#0d1117", border: "1px solid #30363d",
                color: "#58A6FF", fontSize: 11, padding: "5px 12px",
                borderRadius: 20, letterSpacing: "0.05em",
              }}>{tag}</span>
            ))}
          </div>
        </div>
      </div>

      <div style={{ maxWidth: 1000, margin: "0 auto", padding: "56px 24px" }}>

        {/* whoami */}
        <Section title="> whoami">
          <div style={{
            background: "#010409", border: "1px solid #21262d",
            borderRadius: 10, padding: "24px 28px",
            fontFamily: "monospace", fontSize: 13, lineHeight: 2,
          }}>
            {[
              ["name", "Harish Pawar"],
              ["role", "Full Stack Developer"],
              ["focus", "Scalable Web Applications"],
            ].map(([k, v]) => (
              <div key={k}>
                <span style={{ color: "#58A6FF" }}>{k}</span>
                <span style={{ color: "#21262d" }}>{" : "}</span>
                <span style={{ color: "#c9d1d9" }}>{v}</span>
              </div>
            ))}
            <div><span style={{ color: "#58A6FF" }}>strengths</span><span style={{ color: "#21262d" }}> :</span></div>
            {[
              "JWT Authentication & RBAC Systems",
              "Real-Time Backend Architecture",
              "Data Structures & Algorithms (C++)",
              "Production-Level Full Stack Builds",
            ].map(s => (
              <div key={s} style={{ paddingLeft: 20 }}>
                <span style={{ color: "#F05033" }}>- </span>
                <span style={{ color: "#c9d1d9" }}>{s}</span>
              </div>
            ))}
            <div>
              <span style={{ color: "#58A6FF" }}>status</span>
              <span style={{ color: "#21262d" }}> : </span>
              <span style={{ color: "#47A248" }}>Open to opportunities ✦</span>
            </div>
          </div>
        </Section>

        {/* Tech Arsenal - Orbit */}
        <Section title="⬡ Tech Arsenal">
          <div style={{ display: "flex", justifyContent: "center" }}>
            <TechOrbit />
          </div>
        </Section>

        {/* Projects */}
        <Section title="◎ Featured Projects">
          <div style={{ display: "flex", gap: 20, flexWrap: "wrap" }}>
            <ProjectCard
              emoji="🚓"
              title="E-FIR Portal"
              subtitle="A production-grade citizen-facing law enforcement platform"
              points={[
                "JWT-based stateless authentication",
                "Role-Based Access — Citizen / Admin",
                "PostgreSQL relational data modeling",
                "Analytics dashboard with live metrics",
                "Fully responsive Tailwind CSS UI",
              ]}
              badges={["React", "Node.js", "PostgreSQL", "JWT", "TailwindCSS"]}
            />
            <ProjectCard
              emoji="🏥"
              title="Real-Time Emergency Triage"
              subtitle="Priority-driven patient management at scale"
              points={[
                "Heap-based priority queue (custom DSA)",
                "Real-time dashboard — live updates",
                "Role system — Doctor / Receptionist",
                "Full frontend + backend integration",
                "Edge-case safe queue management",
              ]}
              badges={["React", "Express.js", "DSA", "Heap Logic", "Real-Time"]}
            />
          </div>
        </Section>

        {/* GitHub Metrics */}
        <Section title="◈ GitHub Metrics">
          <div style={{ display: "flex", gap: 16, marginBottom: 20, flexWrap: "wrap" }}>
            <StatCard label="Public Repos" value="12+" sub="pawarharish04" />
            <StatCard label="Primary Stack" value="JS/C++" sub="Full Stack" />
            <StatCard label="Focus" value="DSA" sub="Algorithms & Systems" />
            <StatCard label="Status" value="🟢" sub="Open to Work" />
          </div>
          <div style={{
            background: "#010409", border: "1px solid #21262d", borderRadius: 10,
            overflow: "hidden",
          }}>
            <img
              src="https://github-readme-activity-graph.vercel.app/graph?username=pawarharish04&bg_color=010409&color=58A6FF&line=58A6FF&point=ffffff&area=true&hide_border=true"
              style={{ width: "100%", display: "block" }}
              alt="Activity Graph"
            />
          </div>
          <div style={{ display: "flex", gap: 16, marginTop: 16, flexWrap: "wrap" }}>
            <div style={{ flex: 1, minWidth: 260, background: "#010409", border: "1px solid #21262d", borderRadius: 10, overflow: "hidden" }}>
              <img src="https://github-readme-stats.vercel.app/api?username=pawarharish04&show_icons=true&theme=github_dark&hide_border=true&title_color=58A6FF&icon_color=58A6FF&text_color=c9d1d9&bg_color=010409&count_private=true" style={{ width: "100%", display: "block" }} alt="Stats" />
            </div>
            <div style={{ flex: 1, minWidth: 220, background: "#010409", border: "1px solid #21262d", borderRadius: 10, overflow: "hidden" }}>
              <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=pawarharish04&layout=compact&theme=github_dark&hide_border=true&title_color=58A6FF&text_color=c9d1d9&bg_color=010409" style={{ width: "100%", display: "block" }} alt="Top Langs" />
            </div>
          </div>
        </Section>

        {/* Connect */}
        <Section title="◉ Connect">
          <div style={{ display: "flex", gap: 14, justifyContent: "center", flexWrap: "wrap" }}>
            {[
              { label: "LinkedIn", icon: "🔗", color: "#0077B5", href: "https://www.linkedin.com/in/harish-pawar-8732072b6", sub: "harish-pawar" },
              { label: "Gmail", icon: "✉️", color: "#D14836", href: "mailto:pawarharish899@gmail.com", sub: "pawarharish899" },
              { label: "GitHub", icon: "🐙", color: "#c9d1d9", href: "https://github.com/pawarharish04", sub: "pawarharish04" },
            ].map(link => (
              <a key={link.label} href={link.href} target="_blank" rel="noreferrer" style={{ textDecoration: "none" }}>
                <div style={{
                  background: "#010409",
                  border: `1px solid ${link.color}44`,
                  borderRadius: 10,
                  padding: "16px 28px",
                  textAlign: "center",
                  transition: "all 0.2s",
                  minWidth: 160,
                }}>
                  <div style={{ fontSize: 24 }}>{link.icon}</div>
                  <div style={{ color: link.color, fontWeight: 700, fontSize: 13, marginTop: 6 }}>{link.label}</div>
                  <div style={{ color: "#8b949e", fontSize: 10, marginTop: 2 }}>{link.sub}</div>
                </div>
              </a>
            ))}
          </div>
        </Section>

      </div>

      {/* Footer */}
      <div style={{
        background: "linear-gradient(135deg, #16213e 0%, #1a1a2e 50%, #0a0a0a 100%)",
        borderTop: "1px solid #21262d",
        padding: "32px 24px",
        textAlign: "center",
      }}>
        <p style={{ color: "#58A6FF", fontSize: 14, letterSpacing: "0.3em", margin: 0, fontFamily: "monospace" }}>
          Consistency. Discipline. Growth.
        </p>
      </div>
    </div>
  );
}
