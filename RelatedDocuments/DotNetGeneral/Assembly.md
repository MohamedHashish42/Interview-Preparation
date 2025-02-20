# What is Assembly? 
An assembly is a compiled output (DLL or EXE) that serves as the fundamental unit of deployment, versioning, and execution in .NET.

## File Type
- **Usually a DLL (Dynamic-Link Library):**  For shared libraries that contain code to be used by other programs.  
- **Can also be an EXE (Executable file):**  For standalone programs that can run on their own.  

## Characteristics:
- **Self-describing**: Contains its own metadata.
- **Versionable**: Can have specific version information.
- **Self-contained**: Typically doesn't rely on registry entries

## Contents
- **IL (Intermediate Language) code**
- **Metadata about types, members, and references**: Information about the code structure, not the code itself.
- **Assembly manifest (contains assembly metadata)** : Metadata about the assembly as a whole 
(information like version, culture, security requirements)

## Types:
- **Private assemblies:**: Used by a single application.
- **Shared assemblies:** Can be used by multiple applications.

## Role in .NET
- Provides a way to organize and structure code.
- Enables code reuse across different projects.
- Facilitates versioning and updates.

## Disassembler & Assembler
.NET provides a tool called **ildasm** (Intermediate Language Disassembler) to inspect the contents of an assembly. You can use it to see the code, metadata, and manifest inside the assembly.

Additionally, .NET provides a tool called **ilasm** (Intermediate Language Assembler), which is used to compile Intermediate Language (IL) code into a .NET assembly.
